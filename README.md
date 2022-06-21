## 使用less

vue3 安装stylus
使用less语法

## 安装element-ui


## 引入xlsx插件

1：安装了xlsx模块，但是引入的是undefined
安装的xlsx版本不对

第二个问题：如何自己手动引入到 node_modlues的文件

第三个问题：如果这一列数值没有，读取的数据都没有这个列名
我想要有列名，数据为空就行

sheet: XLSX.utils.sheet_to_json(wb.Sheets[sheetName], {
    //header是设置表属性名，如果设置为数字，则属性名由0，1，2...表示
    //此处设置的header为importHeader:["姓名","年龄"],最终结果的属性名对应该数组
    header: headerRow,
    defval: '',  // 设置默认数据
}),



第四个问题：插入的图片不在这一列，怎么把图片获取到

## 项目请求接口数据


首先在 src\common\Views\basics 路径下面新建一个 同名的 ts文件

animal\src\common\Views\basics\cowInfoManagement\cowInfoManagement.ts

这个ts文件用来向外暴露各种配置

比如你这个vue中需要用到的各种接口类型
然后暴露一个同名class，里面配置各种方法(比如接口请求方法)


然后在vue文件中new 这个类的实例，这样通过实例去调用各种请求方法







## 动态路由配置

1.在系统管理 --> 平台菜单管理  --> 新增栏目
比如我需要添加一个 /cowInfoManagement 的路由

选择对应的菜单打开--> 右边填写 栏目名称，选择父栏目，栏目路径等信息


2.这边添加完毕之后，还需要去 平台权限管理 -> 勾选上刚刚新增的栏目 --> 再选择超级管理员角色


3.如果需要新增请求接口，也要再 第一步的右边新增对应的接口路径

本地: ysk  12345678
外网: ysk  147258369


## 点击图片全屏显示，再次点击退出显示


	<div :class="{ 'show': showMask }"
		class="img-mask" @click="showMask = false">
		<img class="img" :src="form.imgUrl" />
	</div>


.img-mask {
	position: absolute;
	top: 0px;
	left: 0px;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.5);
	z-index: 99;
	display: none;

	&.show {
			display: block;
	}

	.img {
			position: absolute;
			inset: 0px;
			margin: auto;
			height: 80%;
			display: block;
			max-width: 100%;
	}
}




## element el-dialog 嵌套

dialog 嵌套导致蒙层会遮挡住子dialog

在子dialog的标签上加上这两个属性
:modal-append-to-body='false'
:append-to-body='true'

<el-dialog
	:modal-append-to-body='false'
	:append-to-body='true'
></el-dialog>




## 