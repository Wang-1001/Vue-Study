<template>
	<view class="container">
		<view class="pwd-top">
			<text>设置并确认密码</text>
		</view>
		
		<view class="input-box">
			<view class="input-one">
				<input class="uni-input" id="a" password type="text" placeholder="输入密码" v-model="password" required="required" />
			</view>
			<view class="input-one">
				<input class="uni-input" id="b"  password type="text" placeholder="确认密码" v-model="password" required="required" />
			</view>
		</view>
		
		<view class="register-btn">
			<button class="reg-btn"  @tap="signUp(userDTO)" >注册</button>
		</view>
		
		<view class="pwd-bottom">
			<view class="bottom-img">
				<image src="../../static/kkry.png"></image>
			</view>
			<view class="tottom-words">
				<text>这里空空如也 ~ ~ ~</text>
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
	onLoad: function(option) {
		//option为object类型，会序列化上个页面传递的参数
		console.log(option.mobile);
		this.mobile = option.mobile;
	},
	methods: {
		signUp: function(userDTO) {
			var _this = this;
			uni.request({
				url: this.apiServer + '/user/sign_up',
				method: 'POST',
				header: { 'content-type': 'application/json' },
				data: {
					mobile: _this.mobile,
					password: _this.password
				},
				success: res => {
				  	if (res.data.code === 0) {
	                uni.showModal({
						title: '提示',
						content: '注册成功'
					})
					uni.navigateTo({
						url: '../signin/signin'
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
/* 设置并确认密码 */
.pwd-top{
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 15pt;
	margin-top: 5px;
	margin-bottom: 20px;
}	
/* 注册 */
.register-btn{
	
}
.reg-btn{
	display: flex;
	align-items: center;
	justify-content: center;
	width: 80%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
}
/* 底部空白区域 */
.pwd-bottom{
	position: fixed;
	width: 100%;
	bottom: 120px;
	/* border: 1px solid #000000; */
}
.bottom-img image{
	width: 100%;
	/* border: 1px solid #000000; */
}
.tottom-words{
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 10pt;
	color: rgb(134, 128, 128);
}
</style>