<template>
	<view>
		
		<view class="grace-accordion-items">
			<view :class="[accordionActive == 'grace-accordion-2' ? 'grace-accordion-title grace-current' : 'grace-accordion-title']" id="grace-accordion-2" @tap="changeAccordion">
				接收更新推送
				<text class="grace-list-imgs-arrow grace-iconfont icon-arrow-up" v-if="accordionActive == 'grace-accordion-2'"></text>
				<text class="grace-list-imgs-arrow grace-iconfont icon-arrow-down" v-else></text>
			</view>
			<view class="grace-accordion-body" v-if="accordionActive == 'grace-accordion-2'">
				<view class="grace-list" style="border:none;">
					<view class="items"><view class="title">关注的用户</view></view>
					<view class="items"><view class="title">关注的专题</view></view>
					<view class="items"><view class="title">关注的连载</view></view>
					<view class="items"><view class="title">关注的文集</view></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			accordionActive: "grace-accordion-1"
		}
	},
	methods:{
		changeAccordion : function(e){
			var accordionIndex = e.currentTarget.id;
			if (this.accordionActive == accordionIndex){accordionIndex = '';}
			this.accordionActive = accordionIndex;
		}
	}
}
</script>

<style></style>
