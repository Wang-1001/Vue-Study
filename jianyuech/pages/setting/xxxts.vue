<template>
	<view class="container">
		<view class="list-item">
			<view class="option-left">
				<view class="a">
					免打扰
				</view>
				
				<view class="b">
					开启后在设定时间端收到新消将不会震动或响铃
				</view>
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		
		<view class="option-a">
			<view class="option-a-w">
				提醒方式
			</view>
		</view>
		
		<view class="list-item">
			<view class="option-left">
				声音提示
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				震动提示
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		
		<view class="option-a">
			<view class="option-a-w">
				推送项目
			</view>
			
		</view>
		<view class="list-item">
			<view class="option-left">
				<view class="a">
					简书精选
				</view>
				
				<view class="b">
					简书有精选内容时提醒我
				</view>
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				简书钻
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				评论
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				简信
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				投稿建议
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				赞
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				关注
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				赞赏和付费
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		<view class="list-item">
			<view class="option-left">
				其他设置
			</view>
			<view class="option-right">
				<switch color="#E96F5A" />
			</view>
		</view>
		
	</view>
</template>

<script>
</script>

<style scoped>
	.container{
		width: 100%;
	}
.list-item{
	height: 60px;
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.option-left{
	margin-left: 5px;
	font-size: 14pt;
}
.a{
	
	font-size: 14pt;
}
.b{
	font-size: 10pt;
	font-weight: 200;

}
.option-a{
	display: flex;
	align-items: center;
	background: rgb(247,247,247);
	height: 50px;
}
.option-a-w{
	margin-left: 5px;
	font-size: 10pt;
	color: rgb(233, 111, 90);
}
</style>
