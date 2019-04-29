<template>
	<view class="uni-flex uni-column container">
		<view class="name-box">
			<view class="left-box">原昵称</view>
			<view class="right-box">
				{{ nickname }}
			</view>
		</view>
		
		<view class="name-box">
			<view class="left-box">更改昵称</view>
			<view class="right-box">
				<input class="uni-input" type="text" maxlength="5" v-model="renickname" required="required" />
			</view>
		</view>
		
		<view class="amend-btn-box">
			<button class="amend-btn" @tap="changeNickname(renickname)">确认修改</button>
		</view>
		
		<view class="bottom-box">
			<image src="../../static/jstb.png" class="bottom-img"></image>
		</view>
		
		
		
	</view>
</template>

<script>
export default {
	data() {
		return {
			nickname: '',
			renickname: ''
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
		changeNickname: function(renickname) {
			var _this = this;
			uni.request({
				url: 'http://localhost:8080/api/user/nickname?id=' + uni.getStorageSync('login_key').userId,
				method: 'put',
				data: renickname,
				header: { 'content-type': 'application/json' },
				success: res => {
					uni.navigateBack();
				}
			});
		}
	}
};
</script>

<style scoped>
.name-box{
	height:60px ;
	border-bottom: 1px solid rgb(241, 241, 241);
	border-top: 1px solid rgb(241, 241, 241);
	display: flex;
	align-items: center;
	justify-content: space-between;
}	
.left-box{
	display: flex;
	align-items: center;
	margin-left: 5px;
	font-size: 14pt;
}
.right-box{
	margin-right: 10px;
	font-size: 12pt;
	font-weight: 200;
	font-style: italic;
	margin-right: 15px;
}
	
	
input {
	height: 20px;
	width: 80px;
	border-bottom: 1px solid #eee;
	margin-bottom: 5px;
	/* border: 1px solid #000000; */
}
.uni-input{
	font-size: 12pt;
	font-weight: 200;
	font-style: italic;

}
.amend-btn{
	display: flex;
	align-items: center;
	justify-content: center;
	width: 80%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
	
}
.bottom-box{
	display: flex;
	align-items: center;
	justify-content: center;
	height: 200px;
	width: 95%;
    position: fixed;
	bottom: 200px;
	/* border: 1px solid #000000; */
}
.bottom-img{
	width: 100%;
	height: 100%;
}
</style>
