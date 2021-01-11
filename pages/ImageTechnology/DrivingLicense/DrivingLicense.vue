<template>
	<view>
		<u-modal v-model="showModal" :content="content" :mask-close-able="true" @confirm="confirm"></u-modal>
		<view :class="{ wrapper: isShow }" class="content">
			<u-image
				style="width: 80%; height: 350rpx; margin-top: 50rpx;margin-bottom: 50rpx; display: inline-block;vertical-align: middle;"
				v-for="(item, index) in imgArr"
				:key="item.index"
				:src="item"
				mode="aspectFit"
			>
				<u-loading slot="loading"></u-loading>
			</u-image>
		</view>
		<!-- 通过rippleBgColor设置水波纹的背景颜色 -->
		<view style="width: 520rpx;height: 60rpx; padding-top: 32rpx; text-align: center; margin: 0 auto;">
			<u-button type="primary" shape="circle" @click="chooseImg">上传驾驶证主页</u-button>
		</view>
		<view v-if="show">
			<view style="padding-top: 100rpx;padding-left: 20rpx;"><h3>识别结果</h3></view>
			<view style="padding-left: 20rpx;padding-right: 30rpx;">
				<u-form :label-style="form">
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="证 号">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.number"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="姓 名">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.name"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="性 别">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.sex"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="国 籍">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.nation"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="住 址">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.address"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="出 生">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.birthday"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="日 期">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.date"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="车 型">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.type"></u-input>
					</u-form-item>
					<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="期 限">
						<u-input :disabled="true" :border="false" placeholder="" v-model="form.ex_date"></u-input>
					</u-form-item>
				</u-form>
			</view>
			<view style="padding-top: 30rpx;background-color: #f2f2f2;">
				<u-cell-group style="width: 100%;"><u-cell-item title="应用场景" title-width="60rpx" value="司机认证、车主服务" :arrow="false"></u-cell-item></u-cell-group>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			img_src: '',
			imgArr: [],

			form: {
				number: '',
				name: '',
				sex:'',
				nation:'',
				address:'',
				birthday:'',
				date:'',
				type: '',
				ex_date:''
			},
			isShow: true, // 扫描动画是否展示
			show: false, // 是否展示识别结果 设置为false:开始不展示
			showModal: false, // 是否展示提示框
			content: ''
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
					this.imgTobase64(res.tempFilePaths[0]);
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
					client_id: 'nvT2gDQGp5bSySctUwSBhLhV', //  必须参数，自己创建的应用的API Key
					client_secret: 'DMik3VLu0Fid0DWZA3DUadi1QCUiW8uq' // 必须参数，自己创建的应用的Secret Key
				},
				success: res => {
					// console.log('成功获取到token =>', res.data.access_token);
					// 3.token获取成功 -> 调用获取信息方法
					this.getInfo(res.data.access_token);
				}
			});
		},
		// 4.获取信息的方法 api文档：https://ai.baidu.com/ai-doc/OCR/ak3h7xxg3
		getInfo: function(tk) {
			// 发起请求获取信息
			uni.request({
				url: 'https://aip.baidubce.com/rest/2.0/ocr/v1/driving_license?access_token=' + tk,
				// 请求方式
				method: 'POST',
				// 请求头
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				// 请求参数
				data: {
					// 图片信息（图片内容）
					image: this.img_src
				},
				success: res => {
					// 打印获取到的信息
					// console.log('成功获取到信息=>', res.data);
					// 对识别结果进行判断
					this.getJudge(res.data.words_result_num,res.data.words_result);
				}
			});
		},
		getJudge: function(num,data) {
				if(num == 10){
					this.form.number = data.证号.words;
					this.form.name = data.姓名.words;
					this.form.sex = data.性别.words;
					this.form.nation = data.国籍.words;
					this.form.address = data.住址.words;
					let sdate = data.出生日期.words;
					this.form.birthday = sdate.substr(0, 4) + '年' + sdate.substr(4, 2) + '月' + sdate.substr(6, 2) + '日';
					let date = data.初次领证日期.words;
					this.form.date = date.substr(0, 4) + '年' + date.substr(4, 2) + '月' + date.substr(6, 2) + '日';
					this.form.type = data.准驾车型.words;
					let exdate = data.有效期限.words;
					this.form.ex_date = exdate.substr(0, 4) + '年' + exdate.substr(4, 2) + '月' + exdate.substr(6, 2) + '日';
					// 展示识别结果
					this.show = true;
					// 隐藏扫描动画
					this.isShow = false;
				}else{
					// 展示出错信息
					this.showModal = true;
					this.content = '识别有误！';
					
				}
		},
		confirm(){
			uni.redirectTo({
			    url: '/pages/ImageTechnology/DrivingLicense/DrivingLicense'
			});
		}
	}
};
</script>

<style scoped>
.wrapper {
	width: 100%;
	height: 450rpx;
	background: linear-gradient(#2979ff, #2979ff), linear-gradient(90deg, #ffffff33 0.5px, transparent 0, transparent 10px),
		linear-gradient(#ffffff33 0.5px, transparent 0, transparent 10px), linear-gradient(transparent, #2979ff);
	background-size: 100% 1.5%, 1.5% 100%, 100% 4%, 100% 100%;
	background-repeat: no-repeat, repeat, repeat, no-repeat;
	background-position: 0% 0%, 0 0, 0 0, 0 0;
	/* 初始位置 */
	clip-path: polygon(0% 0%, 100% 0%, 100% 1.5%, 0% 1.5%);
	/* 添加动画效果 */
	animation: move 2s infinite linear;
}
@keyframes move {
	to {
		background-position: 0 100%, 0 0, 0 0, 0 0;
		/* 终止位置 */
		clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%);
	}
}

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
