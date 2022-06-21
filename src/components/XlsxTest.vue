<template>
  <div class="wrap">
    <div class="title">
        导入excel
    </div>

    <div class="upload">
        <el-upload
          ref="upload"
          action="/"
          :show-file-list="false"
          :on-change="fileChange"
          :auto-upload="false"
          style="margin-left: 30px;width:130px">
          <el-button
            style="width: 130px"
            type="primary"
            plain
            icon="el-icon-upload2"
            class="handle-del">批量导入
          </el-button>
        </el-upload>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue';
import XLSX from 'xlsx'

export default defineComponent({
  name: 'HelloWorld',
  props: {
    msg: String,
  },

  setup() {
    let xlsxJson: any = reactive({})
    // const importHeader = reactive(
    //   ['登记场',	'品种',	'出生日期',	'使用类型',	'牛只来源',	'简介',	'初生重',
    //   '6月龄重',	'12月龄重',	'18月龄重',	'24月龄重',	'父号',	'母号',	'祖父',	'祖母',
    //   '曾祖父',	'曾祖母',	'曾外祖父',	'曾外祖母',	'外祖父',	'外祖母',	
    //   '外曾祖父',	'外曾祖母',	'外曾外祖父',	'外曾外祖母',
    //   ])
      const importHeader1: any = []
    let msg = ref('Welcome to Your Vue.js App')
    let wb: any = reactive({})
    
    const fileChange = (file: any, fileList:any) => {
      batchImport(file, fileList, importHeader1)
    }

    const batchImport = (file: any, hfileList:any , header: any) => {
      // let file = file.files[0] // 使用传统的input方法需要加上这一步
      const types = file.name.split(".")[1];
      const fileType = ["xlsx", "xlc", "xlm", "xls", "xlt", "xlw", "csv"].some(
        (item) => item === types
      );
      if (!fileType) {
        alert("格式错误！请重新选择");
        return;
      }

      file2Xce(file,header).then((tabJson: any) => {
        if (tabJson && tabJson.length > 0) {
          xlsxJson = tabJson;
        }
        setLedgerList();
      });
    }

    const setLedgerList = () => {
      let data: any[] = [];

      xlsxJson[0].sheet.forEach((item: any) => {
        data.push(item);
      });
      console.log(data)
    //  这个data就可以传给后端，存入数据了
    }

    const getHeaderRow = (sheet: any) => {
        console.log('获取excel的表头作为列名', sheet)
        const headers = [];
        /* sheet['!ref']表示所有单元格的范围，例如从A1到F8则记录为 A1:F8*/
        const range = XLSX.utils.decode_range(sheet['!ref']);
        let C, R = range.s.r; /* 从第一行开始 */
        /* 按列进行数据遍历 */
        for (C = range.s.c; C <= range.e.c; ++C) {
            /* 查找第一行中的单元格 */
            const cell = sheet[XLSX.utils.encode_cell({c: C, r: R})] 

            let hdr = "UNKNOWN " + C; // <-- 进行默认值设置
            if (cell && cell.t) hdr = XLSX.utils.format_cell(cell);

            headers.push(hdr);
        }
        return headers;
    }

    const file2Xce = (file: any,header: any) => {
      return new Promise(function (resolve, reject) {
        const reader = new FileReader();

        console.log('new FileReader', reader)

        reader.onload = function (e) {
          const data = e.target!.result;
          console.log('xlsx-data:', e)
          console.log('XLSX 插件:', XLSX)
          // XLSX.read返回值为WorkBook对象，包含整个文件的所有表
          wb = XLSX.read(data, {
            type: "binary",
          });

          console.log('wb-data', JSON.parse(JSON.stringify(wb)))
          console.log('wb-data-string', JSON.stringify(wb))
          const result: any = [];
          const headerRow = getHeaderRow(wb.Sheets[wb.SheetNames[0]])
          //SheetNames包含了文件中所有的表明
          wb.SheetNames.forEach((sheetName: any) => {
            result.push({
              sheetName: sheetName,
              sheet: XLSX.utils.sheet_to_json(wb.Sheets[sheetName], {
                //header是设置表属性名，如果设置为数字，则属性名由0，1，2...表示
                //此处设置的header为importHeader:["姓名","年龄"],最终结果的属性名对应该数组
                header: headerRow,
                defval: '',
              }),
            });
          });
          console.log("result")
          console.log(result)
          //将excel文件第一张表的第一项(excel的第一行为属性名，应该去掉)删除
          console.log(result[0].sheet)
          result[0].sheet.shift();
          resolve(result);
        };
        reader.readAsBinaryString(file.raw);
        // reader.readAsBinaryString(file) // 传统input方法
      });
    }

    return {
      fileChange
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
.wrap {
    height: 100%;
    width: 100%;

    .title {
        height: 200px;
        width: 100%;
        text-align: left;
    }
}
</style>
