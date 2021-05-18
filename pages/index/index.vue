<template>
	<view class="content">
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="text-info">
			<text>uTools 是一个极简、插件化的现代桌面软件，通过自由选配丰富的插件，打造得心应手的工具集合。

				通过快捷键（默认 alt + space ）就可以快速呼出这个搜索框。你可以往输入框内粘贴文本、图片、截图、文件、文件夹等等，能够处理此内容的插件也早已准备就绪，统一的设计风格和操作方式，助你高效的得到结果。

				一旦你熟悉它后，能够为你节约大量时间，即用即走、不中断、无干扰，让你可以更加专注地改变世界。</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'uTools 是什么？'
			}
		},
		onLoad() {
			this.getJsapiTicket();
		},
		methods: {
			getJsapiTicket(){
				const that = this;
				uni.request({
				    url: '/wx/mp/jsapi/getJsapiTicket', //仅为示例，并非真实接口地址。
					data:{
						url:window.location.href
					},
				    success: (res) => {
				        console.log(res.data);
						that.initWxJsdk(res.data)
				    }
				});
			},
			initWxJsdk(config){
				jWeixin.config({
					...config,
				  debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
				  jsApiList: ["updateAppMessageShareData","updateTimelineShareData"] // 必填，需要使用的JS接口列表
				});
				this.initWxShare(this.title)
			},
			initWxShare(title) {
				const config = {
					title,
					desc: "哈哈哈",
					link:window.location.href,
					imgUrl: 'https://img0.baidu.com/it/u=233301930,3031623456&fm=11&fmt=auto&gp=0.jpg', // 分享图标

				}
				console.log(config);
				jWeixin.ready(() => { //需在用户可能点击分享按钮前就先调用
					jWeixin.updateAppMessageShareData({
						...config,
						success: () => {
							console.log("设置成功")
						}
					})
					jWeixin.updateTimelineShareData({
						...config,
						success: function() {
							console.log("设置成功")
						}
					})
				});
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
		padding: 60rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}

	.text-info {
		padding-top: 60rpx;
	}
</style>
