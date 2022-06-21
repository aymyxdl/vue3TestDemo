<template>
  <div class="hello">
    自动轮流播放tooltips标签
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
			tooltip: {
				trigger: 'axis',
				axisPointer: {
					type: 'shadow'
				}
			},
			legend: {},
			grid: {
				left: '3%',
				right: '4%',
				bottom: '3%',
				containLabel: true
			},
			xAxis: [
				{
					type: 'category',
					data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
				}
			],
			yAxis: [
				{
					type: 'value'
				}
			],
			series: [
				{
					name: 'Direct',
					type: 'bar',
					emphasis: {
						focus: 'series'
					},
					data: [320, 332, 301, 334, 390, 330, 320]
				},
				{
					name: 'Email',
					type: 'bar',
					stack: 'Ad',
					emphasis: {
						focus: 'series'
					},
					data: [120, 132, 101, 134, 90, 230, 210]
				},
				{
					name: 'Union Ads',
					type: 'bar',
					stack: 'Ad',
					emphasis: {
						focus: 'series'
					},
					data: [220, 182, 191, 234, 290, 330, 310]
				},
				{
					name: 'Video Ads',
					type: 'bar',
					stack: 'Ad',
					emphasis: {
						focus: 'series'
					},
					data: [150, 232, 201, 154, 190, 330, 410]
				},
				{
					name: 'Search Engine',
					type: 'bar',
					data: [862, 1018, 964, 1026, 1679, 1600, 1570],
					emphasis: {
						focus: 'series'
					},
					markLine: {
						lineStyle: {
							type: 'dashed'
						},
						data: [[{ type: 'min' }, { type: 'max' }]]
					}
				},
				{
					name: 'Baidu',
					type: 'bar',
					barWidth: 5,
					stack: 'Search Engine',
					emphasis: {
						focus: 'series'
					},
					data: [620, 732, 701, 734, 1090, 1130, 1120]
				},
				{
					name: 'Google',
					type: 'bar',
					stack: 'Search Engine',
					emphasis: {
						focus: 'series'
					},
					data: [120, 132, 101, 134, 290, 230, 220]
				},
				{
					name: 'Bing',
					type: 'bar',
					stack: 'Search Engine',
					emphasis: {
						focus: 'series'
					},
					data: [60, 72, 71, 74, 190, 130, 110]
				},
				{
					name: 'Others',
					type: 'bar',
					stack: 'Search Engine',
					emphasis: {
						focus: 'series'
					},
					data: [62, 82, 91, 84, 109, 110, 120]
				}
			]
		};

		var index = 0; //播放所在下标
		var mTime = setInterval(function() {
			myChart.dispatchAction({
				type: 'showTip',
				seriesIndex: 0,
				dataIndex: index
			});
			index++;
			if(index > option.xAxis[0].data.length) {
				index = 0;
			}
		}, 3000);

		// 注意：DISPATCHACTION 方法是关联到 OPTION的TOOLTIP项，在OPTION里一定要配置TOOLTIP

  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
