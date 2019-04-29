<template>
	<view class="container">
		<!-- 顶部扫一扫 日夜间切换 -->
		<view class="roof">
			<view class="roof-left"><image src="../../static/sys.png"></image></view>
			<view class="roof-right" v-if="followed" @tap="handclick">
				<view class="roof-right-img"><image src="../../static/rj.png"></image></view>
				<view class="roof-right-words">日间</view>
			</view>
			<view class="roof-right" v-if="!followed" @tap="handclick">
				<view class="roof-right-img"><image src="../../static/yj.png"></image></view>
				<view class="roof-right-words">夜间</view>
			</view>
		</view>

		<!-- 用户信息 -->
		<view class="top">
			<view class="avatar-box">
				<view class="avatar-box-s"><image src="../../static/default.png" mode="scaleToFill" class="avatar" v-if="!storageData.login"></image></view>

				<view class="avatar-box-s">
					<navigator url="../personal-center/personal-center" v-if="storageData.login">
						<image :src="avatar" mode="scaleToFill" class="avatar" v-if="storageData.login"></image>
					</navigator>
				</view>
			</view>
			<view class="info-box" v-if="!storageData.login">
				<view class="info-box-left">
					<view class="info-box-m"><navigator url="../signin/signin" class="info-box-ws">点击登录</navigator></view>
					<view class="info-box-w">立即赢取简书钻福利</view>
				</view>
				<view class="info-box-right"><image src="../../static/jstb.png"></image></view>
			</view>
			<view class="box-info" v-if="storageData.login">
				<view class="box-info-left">
					<view class="user-name">{{ nickname }}</view>
					<view class="user-af">
						<view class="user">文章 {{ articles.length }}</view>
						<view class="user">关注 {{ follows.length }}</view>
						<view class="user">粉丝 {{ like.length }}</view>
					</view>
				</view>
				<view class="box-info-right"><navigator url="../setting/setting" v-if="storageData.login" class="btn">个人设置</navigator></view>
			</view>
		</view>

		<!-- 滚动图片区域-->
		<view class="photo">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="carousel">
				<swiper-item><image src="../../static/11.png" class="photo-img"></image></swiper-item>
				<swiper-item><image src="../../static/111.png" class="photo-img"></image></swiper-item>
				<swiper-item><image src="../../static/2.jpg" class="photo-img"></image></swiper-item>
				<swiper-item><image src="../../static/3.jpg" class="photo-img"></image></swiper-item>
			</swiper>
		</view>

		<!-- 个人-->
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
						
						
							
							
								<uni-swipe-action class="list-item" v-on:click="deletArticles(article.id)" :options="options" v-for="(article, index) in articles" :key="index">
								<view class="list-title">
									<text @tap="gotoDetail(article.id)">{{ article.title }}</text>
								</view>
								<view class="list-time">{{ handleTime(article.createTime) }}</view>
								</uni-swipe-action>
							
								
							
						
						
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
							<view style="padding:25px; padding-bottom:30px;">
								<button type="warn" style="background:#F6644D; padding:0 20px;" @tap="closeBanner">签到成功</button>
							</view>
						</graceMaskView>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import uniSwipeAction from '@/components/uni-swipe-action/uni-swipe-action.vue';
import graceMaskView from '../../graceUI/components/graceMaskView.vue';
var loginRes, _self;
/*  */
var _self, x, y;
export default {
	components: {
		uniSwipeAction
	},

	data() {
		return {
			// /*  */
			// //2个按钮的宽度
			// btn1Width : 70,
			// btn2Width : 70,
			// //2个按钮的总宽 = btn1Width + btn23Width【uni-app 模板内不建议复杂运算请手动配置】
			// deleteBtnWidth : 140,
			// //正在滑动的索引
			// scrollIndex : -1,
			// //正在删除的索引
			// deleteIndex : -1,
			options: [
				{
					text: '删除',
					style: {
						backgroundColor: '#dd524d'
					}
				}
			],

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
					name: '粉丝'
				},
				{
					cateid: 3,
					name: '收藏'
				},
				{
					cateid: 4,
					name: '签到'
				}
			],
			// 当前选择的分类
			cateCurrentIndex: 0,
			articles: [],
			follows: [],
			like: [],

			/* 遮罩层 */
			staticUrl: this.staticUrl,
			show: false,
			show2: false,
			followed: true
		};
	},
	/* onLoad: function() {}*/
	onLoad: function() {
		_self = this;
	},
	onPullDownRefresh:function(){
		this.deletArticles();
	},
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
		deletArticles: function(id) {
			var _this = this;
			uni.request({
				method: 'DELETE',
				url: 'http://localhost:8080/api/article/user?id=' + id,
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				success: res => {
				
				},
				complete: function() {
					uni.stopPullDownRefresh();
				}
			});
		},

		handclick: function() {
			this.followed = !this.followed;
		},
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
		showBanner: function() {
			this.show = true;
		},
		closeBanner: function() {
			this.show = false;
		},
		// 第2个演示 开启与关闭
		showBanner2: function() {
			this.show2 = true;
		},
		closeBanner2: function() {
			this.show2 = false;
		},
		tap2: function() {
			uni.showToast({
				title: '您点击了红包图片',
				icon: 'none'
			});
		}

		// /*  */
		// removeMsg : function(e){
		// 	var index = e.target.dataset.id;
		// 	//删除确认
		// 	if (this.deleteIndex != index){
		// 		this.deleteIndex = index;
		// 		setTimeout(function(){
		// 			_self.deleteIndex = -1;
		// 		}, 2000);
		// 		return false;
		// 	}
		// 	this.scrollIndex = -1;
		// 	setTimeout(function(){
		// 		_self.msg.splice(index, 1);
		// 		_self.deleteIndex = -1;
		// 	}, 500);
		// },
		// touchStart: function (e) {
		// 	x = e.mp.changedTouches[0].clientX;
		// 	y = e.mp.changedTouches[0].clientY;
		// },
		// touchEnd : function(e){
		// 	x = x - e.mp.changedTouches[0].clientX;
		// 	y = y - e.mp.changedTouches[0].clientY;
		// 	if (Math.abs(x) < 50) { return; }
		// 	if (Math.abs(x) < Math.abs(y)) { return; }
		// 	var index = e.currentTarget.dataset.id;
		// 	if (index == this.scrollIndex){return ;}
		// 	_self.scrollIndex = index;
		// },
		// changStatus : function(e){
		// 	var index = e.currentTarget.dataset.id;
		// 	this.msg[index].status = "已读";
		// }
	},
	/* 遮罩层 */
	components: {
		graceMaskView
	}
};
</script>

<style scoped>
.container {
	background: rgb(247, 247, 247);
	width: 100%;
}
/* 顶部扫一扫 日夜间切换 */
.roof {
	margin-left: 5px;
	margin-right: 5px;
	display: flex;
	justify-content: space-between;
	align-items: center;
	background: rgb(250, 250, 250);
}
.roof image {
	width: 20px;
	height: 20px;
}
.roof-right {
	display: flex;
	align-items: center;
	justify-content: center;
}
.roof-right-img {
	display: flex;
	justify-content: center;
	align-items: center;
}
.roof-right-words {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-left: 5px;
	font-size: 8pt;
}
/* 用户信息 */
.top {
	display: flex;
	align-items: center;
	height: 70px;
	margin-top: 10px;
	background: rgb(250, 250, 250);
	/* border: 2px solid #007AFF; */
}
.avatar-box {
	margin-left: 5px;
	display: flex;
	align-items: center;
	justify-content: center;
}
/* 未登录文字设置 */
.info-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-left: 10px;
	width: 100%;
	/* border: 1px solid blue; */
}
.info-box-ws {
	font-size: 15pt;
}
.info-box-w {
	margin-top: 3px;
	font-size: 9pt;
	font-weight: 200;
	color: rgb(167, 167, 167);
}
.info-box-right {
	height: 70px;
	width: 100px;
	/* border: 1px solid #00B26A; */
}
.info-box-right image {
	width: 100%;
	height: 100%;
}
/* 登录后 */
.box-info {
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-left: 10px;
	width: 100%;
}
.user-name {
	font-size: 15pt;
}
.user-af {
	display: flex;
	align-items: center;
}
.user {
	margin-top: 3px;
	font-size: 9pt;
	font-weight: 200;
	color: rgb(167, 167, 167);
	margin-right: 15px;
}
/*  个人设置*/
.box-info-right {
	margin-right: 10px;
}
.btn {
	margin-left: 10px;
	color: rgb(133, 218, 70);
}
/* 图片滚动 */
.photo {
	margin-left: 5px;
	margin-right: 5px;
	margin-top: 20px;
	margin-bottom: 10px;
	border-radius: 10px;
}
.carousel {
	width: 100%;
	height: 100px;
	border-radius: 5px;
	/* border:  1px solid #000000; */
}
.photo-img {
	width: 100%;
	height: 100px;
	border-radius: 5px;
}

.list-title {
	margin-left: 5px;
	font-size: 12pt;
	font-weight: 800;
}
.list-time {
	margin-right: 5px;
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
	margin-top: 5px;
}
</style>
