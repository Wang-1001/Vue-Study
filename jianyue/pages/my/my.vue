<template>
	<view class="container">
		<view class="top">
			<view class="avatar-box">
				<view class="avatar-box-s">
					<image src="../../static/default.png" 
					mode="scaleToFill" 
					class="avatar" 
					v-if="!storageData.login">
					</image>
					</view>

				
				<view class="avatar-box-s">
					<image :src="avatar" mode="scaleToFill" class="avatar" v-if="storageData.login"></image>
				</view>
				

				
			</view>
			<view class="info-box">
				<view class="info-box-s">
					<navigator url="../signin/signin" v-if="!storageData.login">点击登录</navigator>
					
				
				</view>
				<view></view>
			</view>
		</view>
		<view class="top-name">
			<text v-if="storageData.login">{{ nickname }}</text>
			
		
			
			<navigator url="../setting/setting" v-if="storageData.login" class="btn">
				
				个人设置
			</navigator>
		</view>

		<view class="middle" v-if="storageData.login">
			<view class="card">
				<view class="card-top">{{ articlenum }}</view>
				<view class="card-bottom"><text>文章</text></view>
			</view>
			<view class="card">
				<view class="card-top">{{ attention }}</view>
				<view class="card-bottom"><text>关注</text></view>
			</view>
			<view class="card">
				<view class="card-top">{{ information }}</view>
				<view class="card-bottom"><text>消息</text></view>
			</view>
			<view class="card1">
				<view class="card-top">{{ integral }}</view>
				<view class="card-bottom"><text>积分</text></view>
			</view>
		</view>

		<view class="main" v-if="storageData.login">
			<view class="main-content1" v-for="(article, index) in articles" :key="index">
				<view class="main-content">
					<navigator>{{ article.headline }}</navigator>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
var loginRes, _self;
export default {
	data() {
		return {
			
			storageData: {},
			/* 111 */
			avatar: '',
			nickname: '',
			
			/* 111 */

			articlenum: '10',
			attention: '5',
			information: '66',
			integral: '122',
			articles: [
				{
					id:1,
					headline: '第一篇文章'
				},
				{   
					id:2,
					headline: '第二篇文章'
				},
				{   
					id:3,
					headline: '第三篇文章'
				},
				{   
					id:4,
					headline: '第四篇文章'
				}
			]
		};
	},
	onLoad: function() {},
	/* onShow: function() {
		
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
	}, */
	 onShow: function() {
		var _this = this;
		const loginKey = uni.getStorageSync('login_key');
		
		if (loginKey) {
			// console.log(loginKey);
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
		uni.request({
			url: 'http://localhost:8080/api/user/' + uni.getStorageSync('login_key').userId,
			method: 'GET',
			header: { 'content-type': 'application/json' },
			success: res => {
				if (res.data.code === 0) {
					console.log(res.data.data.avatar+'————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},
	
	methods: {}
};
</script>

<style scoped>
.top {
	/* display: flex; */
	height: 70px;
	margin-top: 10px;
	/* border: 2px solid #007AFF; */
}
.avatar-box {
	display: flex;
	align-content: center;
	justify-content: center;
	/* flex: 1 1 30%; */
	margin-bottom: 15px;
	/* border: 1px solid #aaa; */
}
.info-box {
	/* flex: 1 1 70%; */
	display: flex;
	align-items: center;
	justify-content: center;
	/* border: 1px solid blue; */
}
.top-name {
	display: flex;
	justify-content: center;
	/* border: 1px solid red; */
}
.btn {
	margin-left: 10px;
	/* border: 1px solid red; */
	color: rgb(133, 218, 70);
}
.middle {
	display: flex;
	/* border: 1px solid #007AFF; */
	/* align-content: center; */

	margin-top: 10px;
}
.card {
	flex: 1 1 25%;
	border-right: 1px solid #aaa;
}
.card1 {
	flex: 1 1 25%;
}

.card-top {
	display: flex;
	align-content: center;
	justify-content: center;
}
.card-bottom {
	display: flex;
	align-content: center;
	justify-content: center;
}

.main {
	margin-top: 20px;
	/* border: 1px solid red; */
}
.main-content1 {
	height: 30px;
	border-bottom: 1px solid #aaa;
	/* border: 1px solid black; */
}
.main-content {
	margin-top: 8px;
	margin-left: 5px;
	/* border: 1px solid #007AFF; */
}
</style>
