<template>
	<view class="container">
		<view class="main">
			<view class="input-box">
				<input type="text"  placeholder="在这里输入内容"  />
			</view>
			<view class="btn">
				<button class="submit">发送</button> 
			</view>
		</view>
		
	</view>
</template>

<script>
</script>

<style scoped>

.main{
	display: flex;
	align-items: center;
	position: fixed;
	bottom: 10px;
	width: 100%;
}
.input-box{
	display: flex;
	align-items: center;
	border-radius: 15px;
	background: rgb(240,240,240);
	height: 40px;
	width: 80%;
	border: 1px solid #000000;
}
.submit{
	display: flex;
	align-items: center;
	justify-content: center;
	border-radius: 30px;
	color: rgb(233, 111, 90);
}

</style>
