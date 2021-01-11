<template>
	<view class="content">
		<view><h3>文字识别</h3></view>
		<u-grid :col="3">
			<u-grid-item @click="NavTo('/pages/ImageTechnology/IDCard/IDCard')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">身份证识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/BankCard/BankCard')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">银行卡识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/DrivingLicense/DrivingLicense')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">驾驶证识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/LicensePlate/LicensePlate')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">车牌识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/TrainTicket/TrainTicket')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">火车票识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/AccurateBasic/AccurateBasic')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">通用文字识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/HandWriting/HandWriting')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">手写识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/QrCode/QrCode')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">二维码识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/Numbers/Numbers')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">数字识别</view>
			</u-grid-item>
		</u-grid>
		
		<view><h3>图像识别</h3></view>
		<u-grid :col="3">
			<u-grid-item @click="NavTo('/pages/ImageTechnology/MultiObjectDetect/MultiObjectDetect')">
				<u-icon name="photo" :size="46"></u-icon>
				<view class="grid-text">图像主体检测</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/IDCard/IDCard')">
				<u-icon name="lock" :size="46"></u-icon>
				<view class="grid-text">动物识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/IDCard/IDCard')">
				<u-icon name="hourglass" :size="46"></u-icon>
				<view class="grid-text">植物识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/IDCard/IDCard')">
				<u-icon name="hourglass" :size="46"></u-icon>
				<view class="grid-text">车型识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/IDCard/IDCard')">
				<u-icon name="hourglass" :size="46"></u-icon>
				<view class="grid-text">菜品识别</view>
			</u-grid-item>
			<u-grid-item @click="NavTo('/pages/ImageTechnology/Currency/Currency')">
				<u-icon name="hourglass" :size="46"></u-icon>
				<view class="grid-text">货币识别</view>
			</u-grid-item>
		</u-grid>
	</view>
</template>

<style scoped lang="scss">
.grid-text {
	font-size: 28rpx;
	margin-top: 4rpx;
	color: $u-type-info;
}
</style>

<script>
export default {
	data() {
		return {};
	},
	methods: {
		NavTo(url) {
			uni.navigateTo({
				url
			});
		}
	}
};
</script>
