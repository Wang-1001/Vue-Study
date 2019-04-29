<template>
	<view class="container">
		<view class="top-headline">手机验证码注册</view>

		<view class="call-num"><input class="uni-input" type="number" placeholder="请输入手机号" v-model="mobile" required="required" /></view>

		<view class="auth-code">
			<view class="left"><input class="uni-input" type="text" placeholder="请输入验证码" v-model="verifyCode" required="required" /></view>

			<view class="right">
				<button v-show="show" class="message" @tap="getVerifyCode">获取验证码</button>
				<button v-show="!show" class="message">{{ count }}s后重新获取</button>
			</view>
		</view>

		<view class="next-box">
			<button class="next-btn" @tap="checkCode">下一步</button>
			
		</view>

		<view class="nav-to">
			<navigator url="../signin/signin" hover-class="navigator-hover"><view class="nav-to-left">账号密码登录</view></navigator>
			<view class="line"></view>
			<view class="nav-to-right">登录遇到问题</view>
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
					未注册的手机号登录时将自动注册，且代表您已经同意
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
			show: true,
			count: '',
			timer: null,

			mobile: '',
			verifyCode: ''
		};
	},
	/* onLoad() {
		uni.setNavigationBarTitle({
			title: '注册'
		});
	}, */
	methods: {
		getVerifyCode: function() {
			const TIME_COUNT = 60;

			var _this = this;
			uni.request({
				url: this.apiServer + '/user/verify',
				method: 'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					mobile: _this.mobile
				},
				success: res => {
					if (res.data.code === 0) {
						uni.showToast({
							title: '验证码已发送'
						});
						_this.disabled = true;
						console.log(_this.disabled);
					} else {
						uni.showModal({
							title: '提示',
							content: res.data.msg
						});
					}
				}
			});
			if (!this.timer) {
				this.count = TIME_COUNT;
				this.show = false;
				this.timer = setInterval(() => {
					if (this.count > 0 && this.count <= TIME_COUNT) {
						this.count--;
					} else {
						this.show = true;
						clearInterval(this.timer);
						this.timer = null;
					}
				}, 1000);
			}
		},

		checkCode: function() {
			var _this = this;
			console.log(_this.verifyCode);
			uni.request({
				url: this.apiServer + '/user/check',
				method: 'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					mobile: _this.mobile,
					verifyCode: _this.verifyCode
				},
				success: res => {
					console.log(res.data);
					if (res.data.code === 0) {
						uni.navigateTo({
							url: '../password/password?mobile=' + _this.mobile
						});
					} else {
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
/* 手机验证码注册 */
.top-headline {
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 15pt;
	margin-top: 5px;
	margin-bottom: 20px;
}
/* 文本输入 */
.auth-code {
	display: flex;
	align-items: center;
	margin-bottom: 20px;
	/* border: 1px solid #000000; */
}
.message {
	width: 150px;
	background-color: rgb(233, 111, 90);
	height: 47px;
	color: #ffffff;
	outline: none;
	border-radius: 30px;
	margin-right: -80px;
}
/* 下一步按钮 */
.next-box {
	display: flex;
	align-items: center;
	justify-content: center;
	margin-bottom: 50px;
	/* border: 1px solid #00B26A; */
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
/* 账号密码登录 */
.nav-to {
	display: flex;
	align-items: center;
	justify-content: center;
	color: rgb(123, 140, 165);
}
.nav-to-left {
	margin-right: 5px;
}
.line {
	display: inline-block;
	height: 15px;
	width: 1px;
	background: rgb(191, 191, 191);
}
.nav-to-right {
	margin-left: 5px;
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
