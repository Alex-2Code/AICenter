<template>
	<view class="content">
		<view><h3>语音识别</h3></view>
	<u-grid :col="3">
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">语音识别</view>
		</u-grid-item>
	</u-grid>
	<view><h3>语音合成</h3></view>
	<u-grid :col="3">
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">语音合成</view>
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