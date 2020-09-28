<template>
	<view>
		<button @click="open">打开弹窗</button>
		<uni-popup ref="popup" type="customize">
			<uni-popup-customize @select="select"></uni-popup-customize>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			open() {
				this.$refs.popup.open()
			},
			select: function(){
				console.log(1)
				// this.$refs.popup.close()
			},
		},
		components: {
			uniPopup,
			UniPopupCustomize,
			uniIcons
		}
	}
</script>

<style>

</style>
