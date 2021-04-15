<template>
	<view class="content">
		<view id="olMap" class="map" :style="{'height':mapHieght}">
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				map:undefined,
				mapHieght:'570px',
			}
		},
		methods: {
			
		},
		//页面生命周期-onLoad
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.id); //打印出上个页面传递的参数。
			console.log(option.name); //打印出上个页面传递的参数。
			
			//获取窗体高度
			uni.getSystemInfo({
			    success: (res) => {
				this.mapHieght = res.windowHeight+"px";
			}});
		}
	}
</script>
<script module="olmap" lang="renderjs">
	import "static/js/ol-6.5.0-dist/ol.css";
	let myMap
	export default {
		mounted() {
			if (typeof window.ol === 'function') {
				this.initMap()
			} else {
				// 动态引入较大类库避免影响页面展示
				const script = document.createElement('script')
				// view 层的页面运行在 www 根目录，其相对路径相对于 www 计算
				script.src = 'static/js/ol-6.5.0-dist/ol.js'
				script.onload = this.initMap.bind(this)
				document.head.appendChild(script)
			}
		},
		methods: {
			initMap() {
				// myChart = echarts.init(document.getElementById('echarts'))
				// 观测更新的数据在 view 层可以直接访问到
				// myChart.setOption(this.option)
				console.log('初始化地图')
				this.map = new ol.Map({
					interactions:ol.interaction.defaults({
						pinchRotate:false		//禁止双指旋转地图
					}),
					controls:ol.control.defaults({
						attribution:false,
						zoom:false,
						rotate:false
					}),
					layers: [
						new ol.layer.Tile({
							source: new ol.source.OSM()
						})
					],
					target: "olMap",
					view: new ol.View({
						zoom: 4,
						center: [114, 25],
						projection: "EPSG:3857"
					})
				});
					
				
			},
			updateMap(newValue, oldValue, ownerInstance, instance) {
				// 监听 service 层数据变更
				// myChart.setOption(newValue)
			},
			onClick(event, ownerInstance) {
				// 调用 service 层的方法
				ownerInstance.callMethod('onViewClick', {
					test: 'test'
				})
			}
		}
	}
</script>
<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.map{
		width: 100%;
		height: 600px;
	}
</style>
