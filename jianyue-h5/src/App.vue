<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
	@import 'style/style.css';
	*{
	font-size: 14pt;
}
.container {
	width: 95%;
	margin: 0 auto;
}
.avatar {
	width: 60px;
	height: 60px;
	border-radius: 50%;
}
.list {
	display: flex;
	flex-direction: column;
}
.list-item {
	width: 100%;
	height: 40px;
	background-color: #fff;
	border-bottom: 1px solid #eee;
}
	
	
	
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
