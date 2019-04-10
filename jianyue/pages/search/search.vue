	<template>
	
	<view class="container">
		<view class="search-box">
			<view class="search-text">
				<input type="text" class="search" placeholder="搜索文章/专题/用户/文集" v-model="searchstr" />
			</view>
			<view class="search-icon">
				<image src="../../static/search-grey.png" @tap="search()"></image>
			</view>
		</view>
		
		<view class="main" v-for="(meg,index) in msgs" :key=index v-show="showall" >
			<view class="main-box">
				<view class="box-id">
					{{meg.id}}
				</view>
				<view class="box-title">
					<view class="main-title">
						{{meg.title}}
					</view>
					<view class="main-read">
						<text>阅读</text>
						{{meg.reading}}
					</view>
					
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data () {
			return{
				showall:true,
				searchstr: '',
				
				msgs:[
					{
						id:1,
						title:'每个互联网创业者都应该了解的：10项早起做产品的原则',
						reading:'141'
					},
					{
						id:2,
						title:'有趣或无趣。充实或乏味，全在你一念之间',
						reading:'301'
					},
					{
						id:3,
						title:'人性中的6大怪物，太多现代人沉迷其中，不自知！',
						reading:'474'
					},
					{
						id:4,
						title:'中老年人一定要坚守8大防线',
						reading:'741'
					},
					{
						id:5,
						title:'自然的教育--用布道者的虔诚去传播美的感动',
						reading:'8465'
					},
					{
						id:6,
						title:'詹宁斯诗歌四首，感受不一样的心灵疗救之歌',
						reading:'321'
					},
					{
						id:7,
						title:'每个互联网创业者都应该了解的：10项早起做产品的原则',
						reading:'855'
					},
					{
						id:8,
						title:'每个互联网创业者都应该了解的：10项早起做产品的原则',
						reading:'1267'
					},
					{
						id:9,
						title:'每个互联网创业者都应该了解的：10项早起做产品的原则',
						reading:'874'
					}
					]
			}
		},
		computed: {
			filterArticles: function() {
				var filterArray = this.msgs;
				var searchString = this.searchstr;
				//若搜索关键字为空则返回原始数据
				if (!searchString) {
					return filterArray;
				} else {
					//去除无用字符转换成小写
					searchString = searchString.trim().toLowerCase();
					//过滤数组内容
					filterArray = filterArray.filter(function(item) {
						//找到了标题含有searchString
						if (item.title.toLowerCase().indexOf(searchString) != -1) {
							return item;
						}
					})
					//最终的过滤数组
					return filterArray;
				}
			}
		}
		
		
	}
</script>

<style>
.search-box{
	display: flex;
	justify-content:space-between;
	/* border: 1px solid #00B26A; */
}
.search-text{
	border-radius: 10px;
	width:300px;
	align-content: center;
	border: 1px solid #aaaaaa;
}
.search-icon{
	width:35px ;
	height: 35px;
}
.search-icon image{
	width: 100%;
	height: 100%;
}
.main{
	margin-top: 5px;
}
.main-box{
	margin-bottom: 5px;
	display: flex;
	border-bottom: 1px solid #aaaaaa;
}
.box-id{
	display: flex;
	justify-content: center;
	align-content: center;
}
.main-title{
	font-size: 13pt;
}
.main-read{
	padding-right: 5px;
	font-size: 9pt;
}
</style>
