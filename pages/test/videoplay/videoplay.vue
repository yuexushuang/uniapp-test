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
				isErrorModalShowed:false,
			}
		},
		onReady: function (res) {
			//通过API播放行为的时候需要创建videoContext
			this.videoContext = uni.createVideoContext('myVideo');
			//改变页面标题
			uni.setNavigationBarTitle({
			    title:'测试视频播放'
			})
		},
		methods: {
			videoErrorCallback: function(e) {
				//播放中可能会出现多次错误，要避免多次弹窗
				if(this.isErrorModalShowed){
					return;
				}else{
					this.isErrorModalShowed=true;
					uni.showModal({
						content: "视频传输错误，请返回重试",
						showCancel: false,
						success:(res)=>{
							this.isErrorModalShowed=false;
							if (res.confirm) {
								console.log('用户点击确定');
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}
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
