<template>
	<view class="container">
		<view class="top">
			<view class="top-left">
				<view class="top-left-box">
					<view class="box" v-show="!recommend"><navigator @tap="clickshow()">评论</navigator></view>
					<view class="box navigator" v-show="recommend"><navigator>评论</navigator></view>
				</view>
				<view class="top-left-box">
					<view class="box" v-show="!special"><navigator @tap="clickshow2()">喜欢</navigator></view>
					<view class="box navigator" v-show="special"><navigator>喜欢</navigator></view>
				</view>
				<view class="top-left-box">
					<view class="box" v-show="!serialize"><navigator @tap="clickshow3()">关注</navigator></view>
					<view class="box navigator" v-show="serialize"><navigator>关注</navigator></view>
				</view>
			</view>
			<view class="top-right">
				<view class="search">
					<navigator url="../more/more"><image src="../../static/gd.png"></image></navigator>
				</view>
			</view>
		</view>

		<!-- <view class="content">
			<view class="a">
				<text>评论</text>
			</view>
			<view class="a">
				<text>喜欢和赞</text>
			</view>
			<view class="a">
				<text>关注</text>
			</view>
			<view class="a">
				<text>赞赏和付费</text>
			</view>
			
			
			
			
			
			
		</view> -->
	</view>
</template>

<script>
export default {
	data() {
		return {
			recommend: true,
			special: false,
			serialize: false
		};
	},
	onLoad() {},
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
		}
	}
};
</script>

<style>
/* .content{
	display: flex;
	align-content: center;
	justify-content: center;
}
	.a{
		flex: 1 1 20%;
		font-size: 10pt;
		display: flex;
		align-content: center;
		justify-content: center;
	} */
.container {
	font-size: 13pt;
	background: #eeeeee;
}
.top {
	width: 100%;
	height: 35px;
	background: #ffffff;
	display: flex;
	justify-content: space-between;
	border-bottom: 1px solid #aaa;
	/* border: 1px solid #00B26A; */
}
.top-left {
	margin-left: 3px;
	display: flex;
	width: 50%;
	/* border: 1px solid #007AFF; */
}
.top-left-box {
	height: 100%;
	display: flex;
	flex: 1 1 30%;
}
.top-right {
	margin-right: 10px;
	/* border: 1px solid #007AFF; */
}
.navigator {
	color: #fd572b;
	border-bottom: 2px solid #fd572b;
}
.search image {
	width: 32px;
	height: 32px;
}
</style>
