<template>
  <div class="hello">
    柱状体自动右滑
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
		let echarts: any = null
		let chartDom = document.getElementById('main');
		let myChart = echarts.init(chartDom);
		let option: any = null

		option = {
			dataZoom: [
				{ // 第一个 dataZoom 组件
					type: 'inside',
					xAxisIndex: 0, // 表示这个 dataZoom 组件控制 第一个 xAxis
					startValue: 0, // 数据窗口范围的起始数值index
					endValue: 5, // 数据窗口范围的结束数值index
				},
			],
			xAxis: {
				type: 'category',
				data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun', '123', '2', '3', '4', '5', '6', '7']
			},
			yAxis: {
				type: 'value'
			},
			series: [
				{
					data: [120, 200, 150, 80, 70, 110, 130, 200, 170, 211, 69, 125, 199, 226],
					type: 'bar'
				}
			],
    };

    let num = null
    function auto() {
			// 设置定时器, 每隔2s判断是否滚动到末尾, 是则重置为初始状态, 否则向前滚动一位
			num = setInterval(() => {
					if (option.dataZoom[0].endValue == option.series[0].data.length - 1) {
							option.dataZoom[0].endValue = 5
							option.dataZoom[0].startValue = 0
					} else {
							option.dataZoom[0].endValue =
									option.dataZoom[0].endValue + 1
							option.dataZoom[0].startValue =
									option.dataZoom[0].startValue + 1
					}
					myChart.setOption(option)
			}, 2000)
    }

    auto()

    myChart.on('mouseover', () => {
    	clearInterval(num)
    })
    myChart.on('mouseout', () => {
    	auto()
    })

	option && myChart.setOption(option);
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
