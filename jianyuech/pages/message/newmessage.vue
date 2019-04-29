<template>
	<view>
		<view v-for="(value, key) in banner" :key="key">
			<!-- 弹出层 -->
			<view class="uni-banner" style="background:#FFFFFF;" v-if="value.bannerShow">
				<view style="justify-content:flex-end;" @tap="closeBanner" :data-bannerindex="key">
					<view style="justify-content:flex-end; text-align:right; padding:20upx;">
						<text class="uni-icon uni-icon-close"></text>
					</view>
				</view>
				<view>
					<image :src="value.img" style="width:100%;" mode="widthFix"></image>
				</view>
				<view style="padding:50upx 0; padding-bottom:68upx;">
					<button type="warn" class="mini-btn" style="background:#F6644D; margin:0 80upx;">一个按钮</button>
				</view>
			</view>
			<view class="uni-mask" v-if="value.bannerShow"></view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return{
			banner: [
			{
				bannerShow: false,
				img: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg?imageView2/3/w/200/h/100/q/90'
			},
			{
				bannerShow: true,
				img: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/cbd.jpg?imageView2/3/w/200/h/100/q/90'
			},
			{
				bannerShow: true,
				img: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/muwu.jpg?imageView2/3/w/200/h/100/q/90'
			}
		]
		}
		
	},

	methods: {
		closeBanner: function(e) {
			var bannerindex = e.currentTarget.dataset.bannerindex;
			console.log(bannerindex);
			this.banner[bannerindex].bannerShow = false;
		}
	}
};
</script>

<style scoped="">
/* 遮罩层 */
.uni-mask {
	background: rgba(0, 0, 0, 0.5);
	position: fixed;
	width: 100%;
	height: 100%;
	left: 0;
	top: 0;
	z-index: 1;
}

/* 弹出层形式的广告 */
.uni-banner {
	width: 70%;
	position: fixed;
	left: 50%;
	top: 50%;
	background: #fff;
	border-radius: 10upx;
	z-index: 99;
	transform: translate(-50%, -50%);
}
</style>
