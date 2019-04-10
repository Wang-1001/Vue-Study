<template>
	<view class="container">
		<view class="list">
			<view class="option">
				<view class="list-item">文章推送</view>
			</view>
			<view class="option">
				<view class="list-item">消息推送</view>
			</view>
			<view class="option">
				<navigator url="../user_info/user_info">
					<view class="list-item">
						个人资料
					</view>
				</navigator>
			</view>
			<view class="option">
				<view class="list-item">清除缓存</view>
			</view>
			
		</view>
		<button type="primary" @tap="logout" class="green-btn">退出当前账号</button>
	</view>
</template>

<script>
export default {
	data() {
		return {};
	},
	onLoad() {},
	methods: {
		logout: function() {
			console.log('log out');
			uni.removeStorageSync('login_key');
			uni.showToast({
				title: '已经退出当前账号'
			});
			uni.navigateBack();
		}
	}
};
</script>

<style>
.list {
	margin-top: 5px;
}
.option{
	margin-left: 5px;
}
.list-item {
	height: 30px;

	margin-top: 5px;
	

	display: flex;
	align-content: center;
	
}
</style>
