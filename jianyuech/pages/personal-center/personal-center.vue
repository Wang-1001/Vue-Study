<template>
	<view class="container">
		<!-- 顶部图片 -->
		<view class="top-img">
			<image src="../../static/bjt.jpg"></image>
		</view>
		
		<view class="message">
			<!--头像区域  -->
			<view class="message-top">
				<view class="message-top-avatar">
					<image :src="avatar" mode="scaleToFill" class="avatar" ></image>
				</view>
				<view class="message-top-compile">
					<navigator url="../write/jianxin" hover-class="navigator-hover">
						<view class="btn-up">简信</view>
					</navigator>
				</view>
			</view>
			<view class="message-per">
				<view class="name">
					{{ nickname }}
				</view>
				<view class="information">
					<view class="information-box">关注  {{follows.length}}</view>
					<view class="information-box">粉丝  {{like.length}}</view>
					<view class="information-box">简书钻 {{jsz.length}} </view>
				</view>
			</view>
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
					@tap="tabChange">
					{{ cate.name }}
				</view>
			</scroll-view>

			<view class="demo-content" v-if="storageData.login">
				<!-- 文章部分 -->
				
				<view class="content" v-if="cateCurrentIndex === 0">
					
					
					<!-- <scroll-view class="grace-scroll-x" scroll-x v-for="(article, index) in articles" :scroll-left="scrollIndex == index ? 180 : 0" @touchstart='touchStart' @touchend='touchEnd' :data-id="index" scroll-with-animation="true" :key="index">
						<view class="grace-items">
							<view class="list-title">
								<text @tap="gotoDetail(article.id)">{{ article.title }}</text>
							</view>
							<view class="list-time">{{ handleTime(article.createTime) }}</view>
						</view>
						<view class="grace-items btn btn-first" :data-id="index" @tap="changStatus" :style="{width : index == deleteIndex ? '0px' : btn1Width + 'px', background:item.status == '已读' ? '#CCCCCC' : '#5959D3', color:item.status == '已读' ? '#999' : '#FFF'}">{{item.status}}</view>
						<view class="grace-items btn" :data-id="index" @tap="removeMsg" :style="{width : index == deleteIndex ? deleteBtnWidth + 'px' : btn2Width+'px'}">删除</view>
					</scroll-view> -->
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
	import graceMaskView from '../../graceUI/components/graceMaskView.vue';
var loginRes, _self;
/*  */
var _self, x, y;
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
					name: '粉丝'
				
				}
				
			],
			// 当前选择的分类
			cateCurrentIndex: 0,
			articles: [],
			follows: [],
			like:[],
			jsz:[]

			
		};
	},
	/* onLoad: function() {}*/
	onLoad : function(){
		_self = this;
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
		handclick:function(){
						this.followed=!this.followed
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

		
	}
	
	
};
</script>

<style scoped>
.container{
	background: rgb(247,247,247);
	width: 100%;
}
/* 顶部图片 */
.top-img{
	width: 100%;
	height: 125px;
}
.top-img image{
	width: 100%;
	height: 100%;
}
.message{
	background: rgb(255,255,255);
	margin-bottom: 5px;
}
/* 头像区域 */
.message-top{
	display: flex;
	align-items: center;
	justify-content: space-between;
}
/* 头像 */
.message-top-avatar{
	margin-left: 10px;
	margin-top: -30px;
}
.message-top-compile{
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: 3px;
	margin-right: 10px;
	height: 35px;
	width: 70px;
	border-radius: 30px;
	border: 1px solid rgb(90, 178, 77);
}
.btn-up{
	color: rgb(90, 178, 77);
}

/* 个人信息 */

.name{
	margin-left: 10px;
	font-size: 15pt
}
.information{
	display: flex;
	align-items: center;
	padding-bottom: 5px;
	margin-left: 10px;
}
.information-box{
	font-size: 10pt;
	font-weight: 200;
	color: rgb(167, 167, 167);
	margin-right: 15px;
}


/*下部文章 */
.list-title {
	margin-left: 5px;
	font-size: 12pt;
	font-weight: 800;
}
.list-time{
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
