<template>
	<view class="container">
		<view class="list">
			<navigator url="../name_info/name_info">
				<view class="list-item list-item-heigher">
					<view class="left">昵称</view>

					<view class="right">{{ nickname }}</view>
					<view class="list-img">
						<image src="../../static/yjt.png"></image>
					</view>
				</view>
			</navigator>
			<view class="list-item list-item-heigher" @tap="showActionSheet">
				<view class="left">头像</view>
				<view class="right2"><image :src="avatar" class="avatar" ></image></view>
			</view>
			<navigator url="../password_info/password_info">
				<view class="list-item list-item-heigher">
					<view class="left">修改密码</view>
					<view class="list-img">
						<image src="../../static/yjt.png"></image>
					</view>
				</view>
			</navigator>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			/* 1111 */
			nickname: '',
			avatar: '',
			/* 111 */

			/* nickname: uni.getStorageSync('login_key').nickname,
			avatar:uni.getStorageSync('login_key').avatar, */
			userId: uni.getStorageSync('login_key').userId
		};
	},
	onLoad() {},
	onShow() {
		var _this = this;
		uni.request({
			url: 'http://localhost:8080/api/user/' + uni.getStorageSync('login_key').userId,
			method: 'GET',
			header: { 'content-type': 'application/json' },
			success: res => {
				if (res.data.code === 0) {
					console.log(res.data.data.avatar + '————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},
	methods: {
		showActionSheet: function() {
			console.log('show');
			var _this = this;
			uni.showActionSheet({
				itemList: ['拍照', '从相册选择'],
				success: function(res) {
					console.log('选中了第' + (res.tapIndex + 1) + '个按钮');
					//选择的是拍照功能
					if (res.tapIndex == 0) {
						uni.chooseImage({
							count: 1,
							sourceType: ['camera'],
							success: function(res) {
								uni.saveImageToPhotosAlbum({
									filePath: res.tempFilePaths[0],
									success: function() {
										console.log('save success');
										uni.uploadFile({
											url: 'http://localhost:8080/api/user/avatar', //仅为示例，非真实的接口地址
											filePath: res.tempFilePaths[0],
											name: 'file',
											formData: {
												userId: _this.userId
											},
											success: uploadFileRes => {
												console.log(uploadFileRes.data);
												_this.avatar = uploadFileRes.data;
											}
										});
									}
								});
							}
						});
					}
					//从相册选择
					if (res.tapIndex == 1) {
						uni.chooseImage({
							count: 1, //默认9
							sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都
							sourceType: ['album'], //从相册选择
							success: function(res) {
								console.log(JSON.stringify(res.tempFilePaths));
								uni.uploadFile({
									url: 'http://localhost:8080/api/user/avatar', //仅为示例，非真实的接口地址
									filePath: res.tempFilePaths[0],
									name: 'file',
									formData: {
										userId: _this.userId
									},
									success: uploadFileRes => {
										console.log(uploadFileRes.data);
										_this.avatar = uploadFileRes.data;
									}
								});
							}
						});
					}
				},
				fail: function(res) {
					console.log(res.errMsg);
				}
			});
		}
	}
};
</script>

<style>
	
.list-item-heigher {
	height: 60px;
	display: flex;
	align-items: center;
}
.left {
	flex: 1 1 30%;
	margin-top: 20px;
	margin-left: 5px;
	/* border: 1px solid #007AFF; */
}
.right {
	
	flex: 1 1 70%;
	display: flex;
	justify-content: center;
	font-size: 10pt;
	font-weight: 200;
	font-style: italic;
	/* border: 1px solid #DE533A; */
}
.right2 {
	flex: 1 1 70%;
	display: flex;
	justify-content: center;
}

.list-img{
	margin-right: 5px;
}
.list-img image {
	height: 18px;
	width: 18px;
	display: flex;
	align-items: center;
}
</style>
