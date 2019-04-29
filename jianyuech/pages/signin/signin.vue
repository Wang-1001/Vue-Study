<template>
	<view class="uni-flex uni-column container">
		<view class="top-headline">
			账号密码登录
		</view>
		
		<view class="input-box">
			<input
				class="uni-input"
				type="number"
				placeholder="输入手机号"
				v-model="userDTO.mobile"
				required="required"
			/>
			<input
				class="uni-input"
				password
				type="text"
				placeholder="输入密码"
				v-model="userDTO.password"
				required="required"
			/>
		</view>
		
		<view class="register-btn">
			<button class="next-btn" @tap="signIn(userDTO)">登录</button>
		</view>
		
		<view>
			<navigator url="../signup/signup" class="nav">
				<text>还没有账号？点击注册</text> 
			</navigator>
		</view>
		
		
		
		<view class="signup-bottom">
			<view class="bottom-headline">社交账号直接登录</view>
		
			<view class="bottom-other">
				<view class="other-one">
					<view class="one-img"><image src="../../static/wx.png"></image></view>
					<view class="one-text"><text>微信</text></view>
				</view>
				<view class="other-one">
					<view class="one-img"><image src="../../static/qq.png"></image></view>
					<view class="one-text"><text>QQ</text></view>
				</view>
				<view class="other-one">
					<view class="one-img"><image src="../../static/wb.png"></image></view>
					<view class="one-text"><text>微博</text></view>
				</view>
				<view class="other-one">
					<view class="one-img"><image src="../../static/db.png"></image></view>
					<view class="one-text"><text>豆瓣</text></view>
				</view>
			</view>
		
			<view class="bottom-explain">
				<view class="explain-word">
					登录代表您已经同意
				</view>
				
				<view class="explain-word">
					<navigator url="user-agreement" hover-class="navigator-hover"><view class="explain-words">用户协议</view></navigator>
					和
					<navigator url="privacy-policy"><view class="explain-words">隐私政策</view></navigator>
				</view>
				
			</view>
		</view>
		
		
	</view>
</template>

<script>
export default {
	data() {
		return {
			userDTO: {
				mobile: '',
				password: ''
			}
		};
	},
	onLoad() {
		
	},
	methods: {
		signIn: function(userDTO) {
			var _this = this;
			
			uni.request({
				
				url: this.apiServer + '/user/sign_in',
				
				/* url: 'http://localhost:8080/api/user/sign_in', */
				method: 'POST',
				data: {
					mobile: userDTO.mobile,
					password: userDTO.password
				},
				header: {
					'content-type': 'application/json'
				},
				success: res => {
					// console.log(res.data.data);
					if (res.data.code == 0) {
						//将用户数据记录在本地存储
						uni.setStorageSync('login_key', {
							userId: res.data.data.id,
							nickname: res.data.data.nickname,
							avatar: res.data.data.avatar,
							token: res.data.data.token,
							login: true
						});
						uni.showToast({
							title: '登录成功'
						});
						
						uni.switchTab({
							url: '../my/my'
						});
						
					}
					//登录失败，弹出各种原因
					else {
						uni.showModal({
							title: '提示',
							content: res.data.msg
						});
					}
				}
			});
		}
	}
};
</script>

<style scoped>
input {
	height: 50px;
	border-bottom: 1px solid #eee;
	margin-bottom: 5px;
}
/* 账号密码登录 */
.top-headline{
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 15pt;
	margin-top: 5px;
	margin-bottom: 20px;
}
/* 登录按钮 */
.register-btn{
	margin-top: 20px;
	margin-bottom: 20px;
}
.next-btn {
	display: flex;
	align-items: center;
	justify-content: center;
	width: 80%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
}
.nav {
	display: flex;
	justify-content: center;
	color: #00b26a;
	margin-top: 15px;
}


/* 社交账号登录 */
.signup-bottom {
	position: fixed;
	width: 100%;
	bottom: 50px;
}
.bottom-headline {
	position: relative;
	display: flex;
	justify-content: center;
	color: rgb(134, 128, 128);
	font-size: 9pt;
}
.bottom-headline:before {
	content: '';
	position: absolute;
	width: 28px;
	height: 1px;
	top: 50%;
	background-color: rgb(134, 128, 128);
	left: 28%;
}
.bottom-headline:after {
	content: '';
	position: absolute;
	width: 28px;
	height: 1px;
	top: 50%;
	background-color: rgb(134, 128, 128);
	right: 28%;
}
/* 登录方式 */
.bottom-other {
	margin-top: 25px;
	margin-bottom: 25px;
	display: flex;
	align-items: center;
}
.other-one {
	display: flex;
	align-items: center;
	flex: 1 1 20%;
}
.one-img image {
	width: 30px;
	height: 30px;
}
.one-text {
	font-size: 10pt;
	margin-left: 5px;
}
.explain-word{
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 9pt;
}
/* 用户协议 */
.bottom-explain {
	font-size: 9pt;
	color: rgb(134, 128, 128);
}
.explain-words {
	margin-left: 3px;
	margin-right: 3px;
	font-size: 9pt;
	color: rgb(53, 168, 147);
}
</style>