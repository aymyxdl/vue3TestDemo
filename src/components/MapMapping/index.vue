<template>
  <div class="hello">
    地图区域编码映射
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
// import { gdac } from './gaodeV2AreaCode';
import { gaodeV2AreaCode as gdac } from './gaodeV2AreaCode';
import { oldCode } from './oldAreaCode';

import { gaodeAreaCode } from './1.js'
import { gaodeV2AreaCode } from './11.js'
import { echatsAreaCode } from './3.js'

export default defineComponent({
  name: 'MapMapping',
  props: {
    msg: String,
  },
  setup() {

    



    console.log(11111)

    diff()

    function diff() {
      // 这里是用来分析高德地图编码和 我们后台编码不同之处
      const incorrectOjb = {}
      const gaodeonlyhas = {}
      const allHas = {}

      Object.keys(gaodeV2AreaCode).forEach(item => {
        // eslint-disable-next-line
        if (!echatsAreaCode.hasOwnProperty(item)) {
          // 这里记录了高德里面有的，而echart里面没有的
          gaodeonlyhas[item] = gaodeV2AreaCode[item]
        } else {
          // 当两边都有这个key时，看看name是不是一样
          let gdresult = gaodeV2AreaCode[item]
          let ecresult = echatsAreaCode[item]
          if (gdresult !== ecresult) {
            incorrectOjb[item] = `高德里面的${item}-${gdresult}---echart里面的${item}-${ecresult}`
          } else {
            allHas[item] = gdresult;
          }
        }
        delete echatsAreaCode[item]
      })

      console.log(Object.keys(allHas).length, '两边都有且完全一样的数据', allHas)
      console.log(Object.keys(incorrectOjb).length, '两边都有key，但是name不一样', incorrectOjb);
      console.log(Object.keys(gaodeonlyhas).length, '高德里面有而echart没有', gaodeonlyhas)
      console.log(Object.keys(echatsAreaCode).length, 'echar里面有而高德里面没有', echatsAreaCode)

    }






    // getGaoDeCode()
    // 这个方法是用来将 gdAreaCode.js 这个整体区域编码提取一下
    // 提取出来的结构只有一层结构，所有的区域编码对应的名字都在这里
    function getGaoDeCode () {
      console.log('--------getGaoDeCode start')
      let oldac = JSON.parse(oldCode)
      console.log('gdareacode', gdac);
      console.log('oldAreaCode', oldac);

      const gdResult = {};

      getCode(gdac, gdResult)

      console.log('-----转换后的编码地区对象: ', gdResult);

      function getCode(obj: any, result: any): void {
        result[obj.adcode.toString()] = obj.name
        if (obj?.children?.length) {
          obj.children.forEach(item => {
            getCode(item, result)
          })
        }
      }
    }

    console.log(22222)

  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
