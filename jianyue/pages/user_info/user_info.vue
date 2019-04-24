<template>
	<view class="container">
		<view class="list">
			<view class="list-title"><view class="title-words">常规设置</view></view>

			<view class="list-item-heigher" @tap="showActionSheet">
				<view class="left">更换头像</view>
				<view class="right2"><image :src="avatar" class="avatar"></image></view>
			</view>

			<navigator url="../name_info/name_info">
				<view class="list-item-heigher">
					<view class="left">更改昵称</view>
					<view class="right">
						{{ nickname }}
						<view class="list-img"><image src="../../static/yjt.png"></image></view>
					</view>
				</view>
			</navigator>
		
			<form @submit="formSubmit">
				<view class="list-item-heigher">
					<view class="left">性别</view>
					<view class="right">
						<picker @change="bindPickerChange" :value="genderIndex" :range="gender" name="gender">
							<text>{{ gender[genderIndex] }}</text>
						</picker>
					</view>
				</view>
				<view class="list-item-heigher">
					<view class="left">出生年月</view>
					<view class="right">
						<picker @change="bindDateChange" :value="dateValue" mode="date" name="bd" start="2018-01-01" end="2019-01-01">
							<text>{{ dateValue }}</text>
						</picker>
					</view>
				</view>
				<view class="list-item-heigher"><view class="left">更改主页</view></view>
				<view class="list-item-heigher">
					<view class="left">个人简介</view>
					<view class="right">
						<textarea placeholder="说点什么吧....." maxlength="6" name="desc" class="q"></textarea>
					</view>
				</view>
				<view class="tj">
					<button formType="submit" class="tj-btn">提交</button>
				</view>
			</form>

			<view class="list-title"><view class="title-words">绑定账号登录简书</view></view>
			<view class="title-words-a"><view class="title-words-b">出于安全因素，你至少需要保留一种登录方式</view></view>

			<view class="list-box">
				<view class="list-left"><image src="../../static/sj.png"></image></view>
				<view class="list-right">151****3716</view>
			</view>
			<view class="list-box">
				<view class="list-left"><image src="../../static/wb.png"></image></view>
				<view class="list-right">Beast-1998</view>
			</view>
			<view class="list-box">
				<view class="list-left"><image src="../../static/wx.png"></image></view>
				<view class="list-right">ABC</view>
			</view>
			<view class="list-box">
				<view class="list-left"><image src="../../static/qq.png"></image></view>
				<view class="list-right">已绑定</view>
			</view>
			<view class="list-box">
				<view class="list-left"><image src="../../static/db.png"></image></view>
				<view class="list-right">未绑定</view>
			</view>

			<view class="a"></view>

			<navigator url="../password_info/password_info">
				<view class="list-item-heigher"><view class="left">社交账号是否显示在主页</view></view>
			</navigator>
			<view class="a"></view>
			<navigator url="../password_info/password_info">
				<view class="list-item-heigher"><view class="left">绑定账号遇到问题</view></view>
			</navigator>
			<view class="a"></view>

			<navigator url="../password_info/password_info">
				<view class="list-item-heigher">
					<view class="left">重置密码</view>
					<view class="list-img"><image src="../../static/yjt.png"></image></view>
				</view>
			</navigator>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			genderIndex: 0,
			gender : ['男', '女'],
			dateValue : "请选择",
			

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
		bindPickerChange:function(e){
			console.log(e);
			this.genderIndex = e.detail.value;
		},
		bindDateChange : function(e){
			this.dateValue = e.detail.value;
		},
		formSubmit : function(e){
			wx.showToast({title:'提交成功！', icon:'none'});
			console.log(JSON.stringify(e.detail.value));
		},
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

<style scoped>

.tj-btn{
	display: flex;
	align-items: center;
	justify-content: center;
	width: 40%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
}
.q{
	height: 30px;
	width: 120px;
}
.container {
	width: 100%;
	background: rgb(255, 255, 255);
}
.list-title {
	width: 100%;
	height: 35px;
	display: flex;
	align-items: center;
	background: rgb(252, 252, 252);
}
.title-words {
	margin-left: 5px;
	font-size: 9pt;
	color: rgb(233, 111, 90);
}
.list-item-heigher {
	height: 60px;
	display: flex;
	align-items: center;
	justify-content: space-between;
	border-bottom: 1px solid rgb(241, 241, 241);
	border-top: 1px solid rgb(241, 241, 241);
}
.left {
	display: flex;
	align-items: center;
	margin-left: 5px;
	font-size: 14pt;
	/* border: 1px solid #007AFF; */
}
.right2 {
	margin-right: 10px;
	display: flex;
	justify-content: center;
}
.right {
	display: flex;
	justify-content: center;
	font-size: 12pt;
	font-weight: 200;
	font-style: italic;
	margin-right: 15px;
	/* border: 1px solid #007aff; */
}
.list-img {
	
	margin-left: 10px;
}
.list-img image {
	height: 20px;
	width: 20px;
	display: flex;
	align-items: center;
}
.title-words-a {
	background: rgb(252, 252, 252);
	margin-bottom: 10px;
}
.title-words-b {
	margin-left: 5px;
	font-size: 9pt;
	color: rgb(180, 180, 180);
	margin-bottom: 10px;
}
.list-box {
	display: flex;
	height: 60px;
	border-bottom: 1px solid rgb(241, 241, 241);
	border-top: 1px solid rgb(241, 241, 241);
}
.list-left {
	display: flex;
	align-items: center;
	justify-content: center;
	margin-left: 5px;
	margin-right: 20px;
}
.list-left image {
	height: 30px;
	width: 30px;
}
.list-right {
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 14pt;
}
.a {
	height: 20px;
	background: rgb(252, 252, 252);
}
</style>
