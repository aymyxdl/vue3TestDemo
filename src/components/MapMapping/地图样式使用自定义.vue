<template>
  <div class="hello">
    将高德地图的样式设置为自定义
	包含背景颜色，以及线条颜色

	1、如果要设置自定义样式：https://geohub.amap.com/mapstyle/index
		基础模板可以使用自定义5份的样式，够用了，我们用一份就行
	2、选择配色地图： 进入页面进行自定义样式的编辑
	3、编辑成功后，会有一个 在线调用地图样式 地址
		amap://styles/7d38d6d1cfb1bcfdf1f09db5f95ade0f
		这个使用设置，就和样式选择中，极光黑 一样设置
	4、但是设置之后你会发现不生效，这是因为高德官方需要你一定要带上 安全密钥
		window._AMapSecurityConfig = { securityJsCode: '你的js api 的 安全密钥' };
		虽然不推荐这种写法，高德推荐其他更安全的写法，但是这样前端写很方便，就这样将就着用
		然后强制刷新，或者清除缓存，就能看到样式套用生效了。
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'echart',
  props: {
    msg: String,
  },
  setup() {
	  
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
