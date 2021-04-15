<template>
	<view class="content">
		<video id="myVideo" class="my-video"
				src="http://222.141.123.114:8073/IPCamera87/av_stream.flv"
				@error="videoErrorCallback" autoplay
				:show-progress='false'
				:show-play-btn='false'
				:enable-progress-gesture='false'>
		</video>
		<button class="my-button" type="default" @click="fullScreen">全屏</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onReady: function (res) {
			//通过API播放行为的时候需要创建videoContext
			this.videoContext = uni.createVideoContext('myVideo');
		},
		methods: {
			videoErrorCallback: function(e) {
				//播放中可能会出现多次错误，要避免多次弹窗
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			fullScreen(){
				if(this.videoContext){
					this.videoContext.requestFullScreen();
				}
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
	
	.my-video{
		width:100%;
	}
	
	.my-button{
		margin-top: 10px;
	}
</style>
