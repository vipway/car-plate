<template>
	<view class="content">
		<car-plate></car-plate>
	</view>
</template>

<script>
	import carPlate from '../../components/mite-car-plate/mite-car-plate.vue'
	export default {
		components:{
			carPlate
		},
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 400upx;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
</style>
