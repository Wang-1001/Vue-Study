<template>
	<view>
		<view class="pwd-box">
			<input type="text" password placeholder="请输入密码" v-model="changepassword" required="required" value="" />
		</view>
		<view class="pwd-box">
			<input type="text" password placeholder="请确认密码" v-model="changepassword2" required="required" value="" />
		</view>
		<view class="btn-box">
			<button @tap="changePassword(changepassword)" class="btn">确认修改</button>
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
			changepassword: ''
		};
	},
	onLoad() {},
	methods: {
		changePassword: function(changepassword) {
			var _this = this;
			uni.request({
				url: 'http://localhost:8080/api/user/password?id=' + uni.getStorageSync('login_key').userId,
				method: 'put',
				data: changepassword,
				header: { 'content-type': 'application/json' },
				success: res => {
					uni.navigateBack();
				}
			});
		}
	}
};
</script>

<style>
input {
	height: 40px;
	width: 80%;
	border-radius: 5px;
	margin-bottom: 10px;
}
.pwd-box{
	height:60px ;
	border-bottom: 1px solid rgb(241, 241, 241);
	border-top: 1px solid rgb(241, 241, 241);
	display: flex;
	align-items: center;
	justify-content: center;
}
.btn {
	display: flex;
	align-items: center;
	justify-content: center;
	width: 80%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
}
/* 底部 */
.pwd-bottom{
	position: fixed;
	width: 100%;
	bottom: 250px;
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
