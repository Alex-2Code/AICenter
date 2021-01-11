<template>
	<view>
		<u-tabs name="cate_name" count="cate_count" :list="list" :is-scroll="false" :current="current" @change="change"></u-tabs>
		<view v-if="current === 0">
			<u-modal v-model="showModal" :content="content" :mask-close-able="true"></u-modal>
			<view :class="{'wrapper':isShow}" class="content">
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
				<u-button type="primary" shape="circle" @click="chooseImg">上传身份证照片页</u-button>
			</view>
			<view v-if="show">
				<view style="padding-top: 100rpx;padding-left: 20rpx;"><h3>识别结果</h3></view>
				<view style="padding-left: 20rpx;padding-right: 30rpx;">
					<u-form :label-style="form">
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="姓  名">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.name"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="性  别">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.sex"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="民  族">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.nation"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="出 生">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.birthday"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="住  址">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.address"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="ID号">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.id"></u-input>
						</u-form-item>
					</u-form>
				</view>
				<view style="padding-top: 30rpx;background-color: #f2f2f2;">
					<u-cell-group style="width: 100%;"><u-cell-item title="应用场景" title-width="60rpx" value="身份认证" :arrow="false"></u-cell-item></u-cell-group>
				</view>
			</view>
		</view>
		<view v-if="current === 1">
			<view :class="{'wrapper':isShow}" class="content">
				<u-image
					style="width: 80%; height: 350rpx; margin-top: 50rpx;margin-bottom: 50rpx; display: inline-block;vertical-align: middle;"
					v-for="(item, index) in imgArrback"
					:key="item.index"
					:src="item"
					mode="aspectFit"
				></u-image>
			</view>
			<!-- 通过rippleBgColor设置水波纹的背景颜色 -->
			<view style="width: 520rpx;height: 60rpx; padding-top: 32rpx; text-align: center; margin: 0 auto;">
				<u-button type="primary" shape="circle" @click="chooseImg">上传身份证国徽页</u-button>
			</view>
			<view v-if="showBack">
				<view style="padding-top: 100rpx;padding-left: 20rpx;"><h3>识别结果</h3></view>
				<view style="padding-left: 20rpx;padding-right: 30rpx;">
					<u-form :label-style="form">
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="单  位">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.authority"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="日  期">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.date"></u-input>
						</u-form-item>
						<u-form-item :rightIconStyle="{ color: '#888', fontSize: '32rpx' }" :border-bottom="true" label="失  效">
							<u-input :disabled="true" :border="false" placeholder="" v-model="form.ex_date"></u-input>
						</u-form-item>
					</u-form>
				</view>
				<view style="padding-top: 30rpx;background-color: #f2f2f2;">
					<u-cell-group style="width: 100%;"><u-cell-item title="应用场景" title-width="60rpx" value="身份认证" :arrow="false"></u-cell-item></u-cell-group>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: [
				{
					cate_name: '照片页'
				},
				{
					cate_name: '国徽页'
				}
			],
			current: 0,
			img_src: '',
			imgArr: [],
			imgArrback: [],
			isShow:true,	// 动画是否展示
			isShowback:true,
			show: false, // 照片页是否展示结果 设置为false:开始不展示
			showBack:false,	// 国徽页是否展示结果 设置为false:开始不展示
			showModal: false,	// 是否展示提示框
			content: '',
			form: {
				name: '', // 姓名
				sex: '', // 性别
				nation: '', // 民族
				birthday: '', // 出生日期
				address: '', // 住址
				id: '', // 身份证号
				authority: '', // 签发机关
				date: '', // 签发日期
				ex_date: '' // 失效日期
			}
		};
	},
	methods: {
		change(index) {
			// 将索引赋给当前页
			this.current = index;
			// 输出当前页的index
			// console.log(index);
		},
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
					// 打印当前页
					// console.log(this.current);
					if(this.current == 0){
					this.imgArr = res.tempFilePaths;
					}else{
					this.imgArrback = res.tempFilePaths;
					}
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
					// 打印出获取到的token
					// console.log('成功获取到token =>', res.data.access_token);
					// 3.token获取成功 -> 调用获取文字信息方法
					this.getInfo(res.data.access_token);
				}
			});
		},
		// 4.获取文字信息的方法 api文档：https://ai.baidu.com/ai-doc/OCR/rk3h7xzck
		getInfo: function(tk) {
			let side = 'front';
			if(this.current ==1)
			{
				side = 'back';
			}
			// 发起请求获取文字信息
			uni.request({
				url: 'https://aip.baidubce.com/rest/2.0/ocr/v1/idcard?access_token=' + tk,
				// 请求方式
				method: 'POST',
				// 请求头
				header: {
					'Content-Type': 'application/x-www-form-urlencoded'
				},
				// 请求参数
				data: {
					// 图片信息（图片内容）
					image: this.img_src,
					// 图片类型
					image_type: 'BASE64',
					id_card_side: side,
				},
				success: res => {
					this.isShow = false;
					// 打印出获取到的数据
					// console.log('成功获取信息=>', res.data);
					let image_status = res.data.image_status;
					if(this.current==0){
					if (image_status == 'normal') {
						// 打印出姓名信息
						// console.log(res.data.words_result.姓名.words);
						// 将获取到的值传给页面绑定的值
						this.form.name = res.data.words_result.姓名.words;
						this.form.sex = res.data.words_result.性别.words;
						this.form.nation = res.data.words_result.民族.words;
						// 对出生字符串进行处理
						let birth = res.data.words_result.出生.words;
						this.form.birthday = birth.substr(0, 4) + '年' + birth.substr(4, 2) + '月' + birth.substr(6, 2) + '日';
						this.form.address = res.data.words_result.住址.words;
						this.form.id = res.data.words_result.公民身份号码.words;
						this.show = true;
						let type = Number(res.data.idcard_number_type);
						switch (type) {
							case -1:
								this.showModal = true;
								this.content = '身份证正面所有字段全为空!';
								break;
							case 0:
								this.showModal = true;
								this.content = '身份证证号识别错误!';
								break;
							case 2:
								this.showModal = true;
								this.content = '身份证证号和性别、出生信息都不一致!';
								break;
							case 3:
								this.showModal = true;
								this.content = ' 身份证证号和出生信息不一致!';
								break;
							case 4:
								this.showModal = true;
								this.content = '身份证证号和性别信息不一致!';
								break;
						}
					} else if (image_status == 'reversed_side') {
						this.showModal = true;
						this.content = '身份证正反面颠倒!';
					} else if (image_status == 'non_idcard') {
						this.showModal = true;
						this.content = '上传的图片中不包含身份证!';
					} else if (image_status == 'blurred') {
						this.showModal = true;
						this.content = '身份证模糊!';
					} else if (image_status == 'other_type_card') {
						this.showModal = true;
						this.content = '其他类型证照!';
					} else if (image_status == 'over_exposure') {
						this.showModal = true;
						this.content = '身份证关键字段反光或过曝!';
					} else if (image_status == 'over_dark') {
						this.showModal = true;
						this.content = '身份证欠曝（亮度过低）!';
					} else {
						this.showModal = true;
						this.content = '未知状态!';
					}
					}else{
						// 打印获取到的数据
						// console.log(res.data);	
						// 将获取到的值传给页面绑定的值
						this.form.authority = res.data.words_result.签发机关.words;
						// 对签发日期字符串进行处理
						let sdate = res.data.words_result.签发日期.words;
						this.form.date = sdate.substr(0, 4) + '年' + sdate.substr(4, 2) + '月' + sdate.substr(6, 2) + '日';
						// 对失效日期字符串进行处理
						let exdate = res.data.words_result.失效日期.words;
						this.form.ex_date = exdate.substr(0, 4) + '年' + exdate.substr(4, 2) + '月' + exdate.substr(6, 2) + '日';
					}
				}
			});
		},
		judge:function(){}
	}
};
</script>

<style>
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
