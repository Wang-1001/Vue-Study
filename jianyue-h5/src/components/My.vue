<template>
	<div class="container">
		<div class="top">
			<div class="avatar-box">
				<img src="../assets/default.png" mode="scaleToFill" class="avatar" v-if="!storageData.login" />
				<img :src="storageData.avatar" mode="scaleToFill"  class="avatar" v-if="storageData.login" />
			</div>
			<div class="info-box">
				<router-link to="/Signin">点击登录</router-link>
				
			</div>
		</div>
	</div>
</template>

<script>
var loginRes, _self;
export default {
	data() {
		return {
			storageData: {}
		};
	},
	onLoad: function() {},
	onShow: function() {
		const loginKey = uni.getStorageSync('login_key');
		console.log('come');
		if (loginKey) {
			console.log(loginKey);
			this.storageData = {
				login: loginKey.login,
				nickname: loginKey.nickname,
				avatar: loginKey.avatar
			};
		} else {
			this.storageData = {
				login: false
			};
		}
	},
	methods: {}
};
</script>

<style>
	
</style>
