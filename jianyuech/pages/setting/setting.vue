<template>
	<view class="container">
		<view class="option">
			<view class="option-list"><view class="list-words">消息推送</view></view>

			<navigator url="wzgxts">
				<view class="list-item"><view class="list-word">文章更新推送</view></view>
			</navigator>

			<navigator url="xxxts">
				<view class="list-item"><view class="list-word">新消息推送</view></view>
			</navigator>
		</view>

		<view class="option">
			<view class="option-list"><view class="list-words">通用设置</view></view>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">编辑个人资料</view></view>
			</navigator>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">默认编辑器</view></view>
			</navigator>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">添加写文章到桌面</view></view>
			</navigator>

			<navigator url="zssz">
				<view class="list-item"><view class="list-word">赞赏设置</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">字号设置</view></view>
			</navigator>
			<navigator url="yszs">
				<view class="list-item"><view class="list-word">隐私设置</view></view>
			</navigator>
			<navigator url="hmdsz">
				<view class="list-item"><view class="list-word">黑名单设置</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">移动网络下加载图片</view></view>
			</navigator>
		</view>

		<view class="option">
			<view class="option-list"><view class="list-words">其他</view></view>

			<navigator url="hsz">
				<view class="list-item"><view class="list-word">回收站</view></view>
			</navigator>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">简书&Yhouse 品质生活卡</view></view>
			</navigator>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">FDAA授权</view></view>
			</navigator>

			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">清除缓存</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">版本更新</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">分享简书</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">给简书好评</view></view>
			</navigator>
			<navigator url="../user_info/user_info">
				<view class="list-item"><view class="list-word">关于我们</view></view>
			</navigator>
		</view>

		
		<view class="secede-box"><button type="primary" @tap="logout" class="secede">退出当前账号</button></view>
	</view>
</template>

<script>
export default {
	data() {
		return {};
	},
	onLoad() {
		uni.setNavigationBarTitle({
			title: '设置'
		});
	},
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

<style scoped>
.container {
	width: 100%;
}
.option {
	
	background: rgb(252, 252, 252);
}
.option-list {
	display: flex;
	align-items: center;
	height: 35px;
	width: 100%;
	color: rgb(233, 111, 90);
}
.list-words {
	font-size: 9pt;
	margin-left: 5px;
}
.list-item{
	display: flex;
	align-items: center;
	height: 60px;
}
.list-word {
	font-size: 14pt;
	margin-left: 5px;
}


/* 退出当前账号 */
.secede {
	display: flex;
	align-items: center;
	justify-content: center;
	width: 80%;
	border-radius: 30px;
	background-color: rgb(233, 111, 90);
	color: #ffffff;
}
</style>
