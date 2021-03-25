<template>
	<view class="Home_Container">
		<view class="">

		</view>
	</view>
</template>

<script>
	export default {
		onShow: function() {
			uni.getLocation({
				type: 'wgs84',
				success: function(res) {
					console.log(res);
					getApp().globalData.latitude = res.latitude;
					getApp().globalData.longitude = res.longitude;
				}
			});
		},
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss">

</style>
