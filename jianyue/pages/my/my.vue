<template>
	<view class="container">
		<!-- 头像 -->
		<view class="top">
			<view class="avatar-box">
				<view class="avatar-box-s"><image src="../../static/default.png" mode="scaleToFill" class="avatar" v-if="!storageData.login"></image></view>

				<view class="avatar-box-s"><image :src="avatar" mode="scaleToFill" class="avatar" v-if="storageData.login"></image></view>
			</view>
			<view class="info-box">
				<view class="info-box-s"><navigator url="../signin/signin" v-if="!storageData.login">点击登录</navigator></view>
			</view>
		</view>
		<!-- 昵称 -->
		<view class="top-name">
			<text v-if="storageData.login">{{ nickname }}</text>

			<navigator url="../setting/setting" v-if="storageData.login" class="btn">个人设置</navigator>
		</view>

		<!-- <scroll-view class="c1" scroll-x="true">
			<view class="c1-1"><image src="../../static/11.png" style="width: 100%;"></image></view>
			<view class="c1-1"><image src="../../static/111.png" mode=""></image></view>
			<view class="c1-1"><image src="../../static/2.jpg" mode=""></image></view>
			<view class="c1-1"><image src="../../static/3.jpg" mode=""></image></view>
		</scroll-view> -->
		<swiper class="grace-swiper" autoplay="true" indicator-dots indicator-color="rgba(255, 255, 255, 1)"
		 indicator-active-color="#00B26A" style="height :240upx " interval="3000">
			<swiper-item v-for="(item, index) in swiperItems" :key="index">
				<navigator :url='item.path' :open-type="item.openType">
					<image :src='item.imgUrl' mode='widthFix' class="MySwiper"></image>
					<view class="title">{{item.title}}</view>
				</navigator>
			</swiper-item>
		</swiper>

		<!-- 个人-->
		<!-- <view class="middle" v-if="storageData.login"> -->
		<view class="middle">
			<scroll-view class="grace-tab-title" scroll-x="true" id="grace-tab-title">
				<view
					v-for="(cate, index) in categories"
					:key="index"
					:data-cateid="cate.cateid"
					:data-index="index"
					:class="[cateCurrentIndex == index ? 'grace-tab-current' : '']"
					@tap="tabChange"
				>
					{{ cate.name }}
				</view>
			</scroll-view>

			<view class="demo-content" v-if="storageData.login">
				<!-- 文章部分 -->
				<view class="content" v-if="cateCurrentIndex === 0">
					<view class="list">
						<view class="list-item" v-for="(article, index) in articles" :key="index">
							<view class="list-title">
								<text @tap="gotoDetail(article.id)">{{ article.title }}</text>
							</view>
							<view class="list-time">{{ handleTime(article.createTime) }}</view>
						</view>
					</view>
				</view>
				<!-- 关注部分 -->
				<view class="content" v-if="cateCurrentIndex === 1">
					<view class="list">
						<view class="list-item" v-for="(follow, index) in follows" :key="index">
							<view class="list-avatar"><image :src="follow.avatar" class="avatar small"></image></view>
							<view class="list-name">
								<text style="margin-left: 20px;">{{ follow.nickname }}</text>
							</view>
						</view>
					</view>
				</view>
				<!-- 收藏部分 -->
				<view class="content" v-if="cateCurrentIndex === 2"><text>收藏</text></view>
				<!-- 积分部分 -->
				<view class="content" v-if="cateCurrentIndex === 3">
					
					<view>
						<graceHeader title="弹出层" desc="请点击按钮测试 ^_^ "></graceHeader>
						<view class="grace-bg-white grace-common-mt grace-padding grace-common-border">
							<view style="padding:200rpx 15%;">
								<button type="primary" @tap="showBanner">点击签到</button>
								<!-- <button type="primary" @tap="showBanner2" style="margin:15px 0;">打开弹出层演示2</button> -->
							</view>
						</view>
						<!-- 弹出层演示 1 -->
						<graceMaskView :show="show" bgcolor="#00C777" v-on:close="closeBanner">
							<view>
								<image
									src="../../static/12.jpg"
									style="width:100%; margin-top:25px; border-top-right-radius:5px; border-top-left-radius:5px;"
									mode="widthFix"
								></image>
							</view>
							<view style="padding:25px; padding-bottom:30px;"><button type="warn" style="background:#F6644D; padding:0 20px;"
							@tap="closeBanner">签到成功</button></view>
						</graceMaskView>
						<!-- 弹出层演示 2 -->
						<!-- <graceMaskView :show="show2" bgcolor="none" v-on:close="closeBanner2">
							<view @tap="tap2"><image src="http://static.hcoder.net/graceui/hb.png" style="width:100%; border-radius:5px;" mode="widthFix"></image></view>
						</graceMaskView> -->
					</view>
					
					
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import graceMaskView from "../../graceUI/components/graceMaskView.vue";
var loginRes, _self;
export default {
	data() {
		return {
			storageData: {
				userId: 0,
				login: false
			},

			/* storageData: {}, */
			avatar: '',
			nickname: '',

			//分类信息
			categories: [
				{
					cateid: 0,
					name: '文章'
				},
				{
					cateid: 1,
					name: '关注'
				},
				{
					cateid: 2,
					name: '收藏'
				},
				{
					cateid: 3,
					name: '签到'
				}
			],
			// 当前选择的分类
			cateCurrentIndex: 0,
			articles: [],
			follows: [],
			
			
			/* 遮罩层 */
			staticUrl: this.staticUrl,
			show : false,
			show2 : false
			
			
		};
	},
	onLoad: function() {},
	onShow: function() {
		var _this = this;
		const loginKey = uni.getStorageSync('login_key');

		if (loginKey) {
			this.storageData = {
				login: loginKey.login,
				nickname: loginKey.nickname,
				avatar: loginKey.avatar,
				userId: loginKey.userId
			};
			/* uni.request({
				
				url: this.apiServer + '/article/user_count',
				method: 'GET',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					userId: this.storageData.userId
				},
				success: res => {
					_this.articleCount = res.data.data;
				}
			}); */
			uni.request({
				url: this.apiServer + '/article/user',
				method: 'GET',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					userId: this.storageData.userId
				},
				success: res => {
					_this.articles = res.data.data;
				}
			});
			uni.request({
				url: this.apiServer + '/follow/list',
				method: 'GET',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					fromUId: this.storageData.userId
				},
				success: res => {
					_this.follows = res.data.data;
				}
			});
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
					console.log(res.data.data.avatar + '————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},

	methods: {
		handleTime: function(date) {
			var d = new Date(date);
			var year = d.getFullYear();
			var month = d.getMonth() + 1;
			var day = d.getDate() < 10 ? '0' + d.getDate() : '' + d.getDate();
			var hour = d.getHours() < 10 ? '0' + d.getHours() : '' + d.getHours();
			var minutes = d.getMinutes() < 10 ? '0' + d.getMinutes() : '' + d.getMinutes();
			var seconds = d.getSeconds() < 10 ? '0' + d.getSeconds() : '' + d.getSeconds();
			return year + '-' + month + '-' + day + ' ' + hour + ':' + minutes + ':' + seconds;
		},
		tabChange: function(e) {
			// 选中的索引
			var index = e.currentTarget.dataset.index;
			// 具体的分类id
			var cateid = e.currentTarget.dataset.cateid;
			this.cateCurrentIndex = index;
			// 动态替换内容
			this.content = this.categories[index].name;
		},
		gotoDetail: function(aId) {
			uni.navigateTo({
				url: '../article_detail/article_detail?aId=' + aId + '&userId=' + this.storageData.userId
			});
		},
		
		/* 遮罩层 */
		// 第1个演示 开启与关闭
		showBanner : function(){
			this.show = true;
		},
		closeBanner : function(){
			this.show = false;
		},
		// 第2个演示 开启与关闭
		showBanner2 : function(){
			this.show2 = true;
		},
		closeBanner2 : function(){
			this.show2 = false;
		}, 
		tap2 : function(){
			uni.showToast({
				title:"您点击了红包图片",
				icon:"none"
			})
		}
		
		
		
	},
	/* 遮罩层 */
	components :{
		graceMaskView
	}
	
};
</script>

<style scoped>
/* 头像 */
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
	margin-bottom: 10px;
	/* border: 1px solid #aaa; */
}
.info-box {
	/* flex: 1 1 70%; */
	display: flex;
	align-items: center;
	justify-content: center;
	/* border: 1px solid blue; */
}
/* 昵称 */
.top-name {
	display: flex;
	justify-content: center;
	/* border: 1px solid red; */
}
/*  个人设置*/
.btn {
	margin-left: 10px;
	/* border: 1px solid red; */
	color: rgb(133, 218, 70);
}

.c1 {
	margin-top: 25px;
	margin-bottom: 10px;
	white-space: nowrap;
	display: flex;
	margin-bottom: 10px;
}
.c1-1 {
	width: 100%;
	height: 130px;
	display: inline-block;
}
.c1-1 image {
	width: 100%;
	height: 100%;
}

.middle {
	margin-top: 10px;
}
.list-title {
	font-size: 12pt;
	font-weight: 800;
}
.list-item {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-right: 5px;

	font-size: 10pt;
	font-weight: 200;
	/* border: 1px solid #007AFF; */
}

.list-avatar {
	margin-left: 20px;
	margin-bottom: 5px;
	height: 30px;
	width: 30px;
}
.list-avatar image {
	width: 100%;
	height: 100%;
}
.list-name {
	margin-right: 5px;
}

.grace-tab-title {
	display: flex;
	/* border:  1px solid #10AEFF; */
}

.grace-tab-box {
	display: flex;
	/* border:  1px solid #007AFF; */
}

.demo-content {
	margin-top: 10px;
}
</style>
