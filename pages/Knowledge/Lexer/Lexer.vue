<template>
	<view>
		
	</view>
</template>

<script>
export default {
	data() {
		return {
			img_src: '',
			imgArr: [],	// 图像数组

			form: {	// 表单数据
				name: ''
			},
			show: true,	// 是否展示识别结果 设置为false:开始不展示
			showModal: false,	// 是否展示提示框
			content: ''	// 内容
		};
	},
	methods: {
		// 1.选择图片的方法
		chooseImg() {
			// 展示扫描动画
			this.isShow = true;
			// 选择图片
			uni.chooseImage({
				// 选择图片的数量
				count: 1,
				sizeType: ['compressed'], //可以指定是原图还是压缩图，默认二者都有
				sourceType: ['album'], //从相册选择
				// 图片选择成功后执行的方法
				success: res => {
					// console.log('成功获取到临时文件路径=>', res.tempFilePaths[0]);
					this.imgArr = res.tempFilePaths;
					// 1.获取到文件的路径 -> 调用图片转Base64的方法
					// this.imgTobase64(res.tempFilePaths[0]);
					// 展示结果
					// this.show = true;
					// 设置扫描动画为不可见
					this.isShow = false;
				}
			});
		},
		// 2.图片转base64的方法
		imgTobase64: function(url) {
			uni.request({
				url: url,
				method: 'GET',
				responseType: 'arraybuffer',
				success: res => {
					// 把转换出来的base64编码，传递到页面的初始化数据中
					this.img_src = uni.arrayBufferToBase64(res.data); //把arraybuffer转成base64
					// 2.base64位转换成功 - 调用获取token的方法
					this.getToken();
				}
			});
		},
		// 3.获取token的方法 api文档：https://ai.baidu.com/ai-doc/REFERENCE/Ck3dwjhhu
		getToken: function() {
			// ajax
			uni.request({
				url: 'https://aip.baidubce.com/oauth/2.0/token',
				method: 'GET',
				header: {
					'Content-Type': 'application/json'
				},
				data: {
					grant_type: 'client_credentials', // 必须参数，固定为client_credentials
					client_id: 'vQMfV9RZl1Zu064hZyE9eiGV', //  必须参数，自己创建的应用的API Key
					client_secret: '12cW40ac7PlRv7O4CpVktCPvDmmvBR9j' // 必须参数，自己创建的应用的Secret Key
				},
				success: res => {
					console.log('成功获取到token =>', res.data.access_token);
					// 3.token获取成功 -> 调用获取图片信息方法
					this.getInfo(res.data.access_token);
				}
			});
		},
		// 4.获取图片信息的方法 api文档：
		getInfo: function(tk) {
			// 发起请求获取信息
			uni.request({
				url: 'https://aip.baidubce.com/rest/2.0/face/v3/detect?access_token=' + tk,
				// 请求方式
				method: 'POST',
				// 请求头
				header: {
					'Content-Type': 'application/json'
				},
				// 请求参数
				data: {
					// 图片信息（图片内容）
					image: this.img_src,
					// 图片类型
					image_type: 'BASE64',
				},
				success: res => {
					console.log('成功获取信息=>', res.data);
				}
			});
		},
		confirm(){
			uni.redirectTo({
			    url: '/pages/index/index'
			});
		}
	}
};
</script>

<style scoped>
.content {
	width: 100%;
	height: 450rpx;
	background-color: #f2f2f2;
	text-align: center;
	margin: 0 auto;
	display: inline-block;
	vertical-align: middle;
}
</style>
