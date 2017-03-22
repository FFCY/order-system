<template>
<div class="goods">
  <div class="menu-wrapper"></div>
  <div class="foods-wrapper"></div>
</div>
</template>
<script type="text/ecmascript-6">
	const ERR_OK = 0;
	export default {
	props: {
	seller: {
	  type: Object
	}
	},
	data() {
	return {
	goods: []
	};
	},
	created() {
	this.$http.get('/api/goods').then((response) => {
	response = response.body;
	if (response.body === ERR_OK) {
	this.goods = response.data;
	}
	});
	}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
.goods{
	display:flex;
	position:absolute;
	width:100%;
	top:170px;bottom:46px;
	overflow:hidden;
}
.menu-wrapper{
	flex:0 0 80px;
	width:80px;
	background:#000;
}
.foods-wrapper{
	flex:1;
	background:#999;
}
</style>
