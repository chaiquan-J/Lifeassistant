<template>
	<view class="content">
		<view class="title">
			{{title}}
		</view>
		<button type="default" @click="select">111</button>
		<view class="close_box" @click="select">
			<!-- <uni-icons class="custom_close" type="close" size="25"></uni-icons> -->
		</view>
	</view>
</template>

<script>
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	export default {
		name: 'UniPopupCustomize',
		props: {
			title: {
				type: String,
				default: '分享到'
			}
		},
		// 直接把下面这一行代码，放到自己的组件内
		inject: ['popup'],
		// ...
		methons: {
			/**
			 * 定义的选择事件，选择内容后触发
			 */
			select: function() {
				// 将事件发送到页面，在页面进行监听
				// this.$emit('select', {
				// 	item,
				// 	index
				// }, () => {
				// 	// 延迟操作，执行父组件的close事件，关闭弹出层
				// 	// this.popup.close()
				// 	console.log(1)
				// })
				this.$emit('select')
			},
			/**
			 * 关闭窗口
			 */
			close: function() {
				// 执行父组件的close事件，关闭弹出层
				// this.$emit('click','')
				// this.popup.close()
				console.log(1)
			}
		},
		components: {
			uniIcons
		}
	}
</script>

<style lang="less">
	.content {
		height: 500rpx;
		width: 100%;
		border-radius: 20rpx 20rpx 0 0;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;

		.title {
			height: 50rpx;
			width: auto;
			background-color: #808080;
			margin-top: 15rpx;
			text-align: center;
		}

		.close_box {
			height: 100rpx;
			width: 100rpx;
			background-color: #007AFF;
			position: absolute;
			top: 20rpx;
			right: 25rpx;
			
			.custom_close {

			}
		}
	}
</style>
