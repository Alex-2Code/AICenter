<template>
	<view class="content">
		<view><h3>自然语言基础技术</h3></view>
	<u-grid :col="3">
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">词法分析</view>
		</u-grid-item>
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">情感倾向分析</view>
		</u-grid-item>
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">短文本相似度</view>
		</u-grid-item>
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">文章标签</view>
		</u-grid-item>
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">评论观点抽取</view>
		</u-grid-item>
	</u-grid>
	<view><h3>文本审核</h3></view>
	<u-grid :col="3">
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">文本审核</view>
		</u-grid-item>
	</u-grid>
	<view><h3>知识理解</h3></view>
	<u-grid :col="3">
		<u-grid-item>
			<u-icon name="photo" :size="46"></u-icon>
			<view class="grid-text">实体标注</view>
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