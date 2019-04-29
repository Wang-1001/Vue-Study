<template>
	<view class="container">
		<view class="top">
			<view class="top-left">
				<view class="top-left-box">
					<view class="box" v-show="!recommend"><navigator @tap="clickshow()">简书钻</navigator></view>
					<view class="box navigator" v-show="recommend"><navigator>简书钻</navigator></view>
				</view>
				<view class="top-left-box">
					<view class="box" v-show="!special"><navigator @tap="clickshow2()">简书贝</navigator></view>
					<view class="box navigator" v-show="special"><navigator>简书贝</navigator></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			recommend: true,
			special: false
		};
	},
	onLoad() {},
	methods: {
		clickshow: function() {
			this.recommend = true;
			this.special = false;
		},
		clickshow2: function() {
			this.recommend = false;
			this.special = true;
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

	border-bottom: 1px solid #aaa;
	/* border: 1px solid #00B26A; */
}
.top-left {
	margin-left: 3px;
	display: flex;

	width: 100%;
	/* border: 1px solid #007AFF; */
}
.top-left-box {
	height: 100%;
	display: flex;
	justify-content: center;
	flex: 1 1 50%;
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
