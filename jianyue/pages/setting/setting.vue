<template>
	<view class="container">
		<view class="list">
			<view class="option">
				<view class="list-item">文章推送</view>
				<view class="list-img"><image src="../../static/yjt.png"></image></view>
			</view>
			<view class="option">
				<view class="list-item">消息推送</view>
				<view class="list-img"><image src="../../static/yjt.png"></image></view>
			</view>
			<navigator url="../user_info/user_info">
				<view class="option">
					<view class="list-item">个人资料</view>

					<view class="list-img"><image src="../../static/yjt.png"></image></view>
				</view>
			</navigator>
			<view class="option">
				<view class="list-item">清除缓存</view>
				<view class="list-img"><image src="../../static/yjt.png"></image></view>
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
.option {
	margin-bottom: 10px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-left: 5px;
}
.list-item {
	height: 30px;
	margin-top: 5px;
	display: flex;
	align-items: center;
	/* border: 1px solid #007AFF; */
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
