<template>
	<view class="container">
		<view class="content" v-for="(other ,index) in others" :key="index">
			<view class="others-content">
				{{other.content}}
			</view>
			<view class="others-time">
				{{other.time}}
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			others:[
				{
					content:'恭喜！您获得了一张简书优惠券',
					time:'2018-12-12 22:22'
				},
				{
					content:'恭喜！您获得了一张简书优惠券',
					time:'2018-12-12 22:22'
				},
				{
					content:'恭喜！您获得了一张简书优惠券',
					time:'2018-12-12 22:22'
				}				
			]
		};
	}
};
</script>

<style>
	.content{
		margin-top: 5px;
		margin-bottom: 5px;
		font-size: 10pt;
		background: #EEEEEE;
		border-bottom: 1px solid #aaa;
	}
	.others-content{
		margin-left: 5px;
		font-size: 12pt;
	}
	.others-time{
		margin-left: 5px;
		font-size: 6pt;
		font-weight: 200;
	}
	
	
</style>
