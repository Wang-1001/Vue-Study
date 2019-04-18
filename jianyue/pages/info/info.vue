<template>
	<view class="container">
		<view class="top">
			<view class="top-left">
				<view class="top-left-box">
					<view class="box" v-show="!recommend"><navigator @tap="clickshow()">推荐</navigator></view>
					<view class="box navigator" v-show="recommend"><navigator>推荐</navigator></view>
				</view>
				<view class="top-left-box">
					<view class="box" v-show="!special"><navigator @tap="clickshow2()">专题</navigator></view>
					<view class="box navigator" v-show="special"><navigator>专题</navigator></view>
				</view>
				<view class="top-left-box">
					<view class="box" v-show="!serialize"><navigator @tap="clickshow3()">连载</navigator></view>
					<view class="box navigator" v-show="serialize"><navigator>连载</navigator></view>
				</view>
			</view>
			<view class="top-right"><image src="../../static/search.png"></image></view>
		</view>

		<view class="article-box">
			<view class="article" v-for="(article, index) in articles" :key="index" v-show="recommend">
				<!-- 标题 -->
				<text class="article-title" @tap="gotoDetail(article.id)">{{ article.title }}</text>
				<!-- 大于等于三张图片的显示方式 -->
				<view class="" v-if="article.imgs.length >= 3">
					<view class="thumbnail-box">
						<view class="thumbnail-item" v-for="(item, index) in article.imgs" :key="index"><image :src="item.url"></image></view>
					</view>
				</view>
				<!-- 小于三种图片的显示方式 -->
				<view class="" v-else-if="article.imgs.length >= 1">
					<view class="text-img-box">
						<view class="left">
							<text>{{ handleContent(article.content) }}</text>
						</view>
						<view class="right"><image :src="article.imgs[article.imgs.length - 1].url"></image></view>
					</view>
				</view>
				<!-- 没有图片的显示方式 -->
				<view class="box" v-else>
					<view class="content1"><rich-text :nodes="handleContent(article.content)" bindtap="tap"></rich-text></view>
				</view>
				<!-- 文章作者等信息 -->
				<view class="article-info">
					<view class="author-avatar"><image :src="article.avatar" class="avatar small"></image></view>
					<view class="author-name">
						<text class="info-text">{{ article.nickname }}</text>
					</view>
					<view class="article-time">
						<text class="info-text">{{ handleTime(article.createTime) }}</text>
					</view>
				</view>
			</view>
		</view>

		<view class="btn">
			<navigator url="../write/write" hover-class="navigator-hover">
				<view class="btn-up"><image src="../../static/pen.png"></image></view>
			</navigator>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			articles: [],
			recommend: true,
			special: false,
			serialize: false
		};
	},
	onLoad: function() {
		this.getArticles();
	},
	onShow: function() {},
	onPullDownRefresh: function() {
		this.getArticles();
	},
	methods: {
		clickshow: function() {
			this.recommend = true;
			this.special = false;
			this.serialize = false;
		},
		clickshow2: function() {
			this.recommend = false;
			this.special = true;
			this.serialize = false;
		},
		clickshow3: function() {
			this.recommend = false;
			this.special = false;
			this.serialize = true;
		},
		getArticles: function() {
			var _this = this;
			uni.request({
				url: this.apiServer + '/article/list',
				method: 'GET',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				success: res => {
					_this.articles = res.data.data;
				},
				complete: function() {
					uni.stopPullDownRefresh();
				}
			});
		},
		gotoDetail: function(aId) {
			uni.navigateTo({
				url: '../article_detail/article_detail?aId=' + aId
			});
		},
		handleTime: function(date) {
			var d = new Date(date);
			var year = d.getFullYear();
			var month = d.getMonth() + 1;
			var day = d.getDate() < 10 ? '0' + d.getDate() : '' + d.getDate();
			var hour = d.getHours() < 10 ? '0' + d.getHours() : '' + d.getHours();
			var minutes = d.getMinutes() < 10 ? '0' + d.getMinutes() : '' + d.getMinutes();
			var seconds = d.getSeconds() < 10 ? '0' + d.getSeconds() : '' + d.getSeconds();
			return year + '-' + day + '' + hour + ':' + minutes + ':' + seconds;
		},

		/* handleContent: function(msg) {
			return msg.substring(0, 50);
		} */
		handleContent: function(msg) {
			let description = msg;
			description = description.replace(/(\n)/g, '');
			description = description.replace(/(\t)/g, '');
			description = description.replace(/(\r)/g, '');
			description = description.replace(/<\/?[^>]*>/g, '');
			description = description.replace(/\s*/g, '');
			return msg.substring(0, 50);
		}
	}
};
</script>

<style scoped>
.top {
	height: 35px;
	background: #ffffff;
	display: flex;
	justify-content: space-between;
	border-bottom: 1px solid #aaa;
	margin-top: 10px;
}
.top-left {
	display: flex;
}
.top-left-box {
	padding-right: 30px;
}
.top-right image {
	width: 30px;
	height: 30px;
}
.article-box {
	/* border: 1px solid #FF3A30 ; */
}
.article {
	margin-bottom: 10px;
	background: #ffffff;
}
.article-title {
	font-size: 13pt;
	font-weight: 700;
}

/* 大于三张 */
.thumbnail-box {
	margin-top: 5px;
	margin-bottom: 5px;
	display: flex;
	flex: 1 1 30%;
	/* border: 1px solid #10AEFF; */
}
.thumbnail-item {
	height: 100px;
	flex: 1 1 30%;
	/* border: 1px solid black; */
}
.thumbnail-item image {
	width: 100%;
	height: 100%;
}
/* 小于三张 */
.text-img-box {
	display: flex;
}
.left {
	flex: 1 1 70%;
}
.right {
	flex: 1 1 30%;
	height: 100px;
}
.right image {
	height: 100%;
	width: 100%;
}
/* 无图 */

.article-text {
	width: 100%;
	/* border:  1px solid #000000; */
}

/* 作者信息 */
.article-info {
	display: flex;
	align-items: center;
	/* border: 1px solid #000000; */
}
.author-avatar {
	margin-right: 15px;
	width: 40px;
	height: 40px;
}
.author-avatar image {
	width: 100%;
	height: 100%;
}
.author-name {
	margin-right: 15px;
	font-size: 11pt;
	font-weight: 400;
}
.article-time {
	font-size: 11pt;
	font-weight: 400;
}

/* 顶部导航栏 */
.container {
	font-size: 13pt;
	background: #eeeeee;
}

/* 底部创建文章按钮 */
.btn-up {
	width: 40px;
	height: 40px;
	border-radius: 50%;
	color: #9370db;
	position: fixed;
	bottom: 60px;
	right: 10px;
}
.btn-up image {
	width: 100%;
	height: 100%;
}
</style>
