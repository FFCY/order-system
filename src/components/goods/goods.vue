<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex==index}" @click="selectMenu(index, $event)">
					<span class="text border-1px">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" class="foods-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
				<ul>
					<li v-for="food in item.foods" class="food-item border-1px">
					<div class="icon">
	                  <img width="57" height="57" :src="food.icon">
	                </div>
					<div class="content">
						<h2 class="name">{{food.name}}</h2>
						<p class="desc">{{food.description}}</p>
						<div class="extra">
							<span class="count">月售{{food.sellCount}}份</span>
							<span>好评率{{food.price}}</span>
						</div>
						<div class="price">
							<span>￥{{food.price}}</span>
							<span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
						</div>
					</div>
					</li>
				</ul>
				</li>
			</ul>
		</div>
	</div>
</template>
<script type="text/ecmascript">
import BScroll from 'better-scroll';
const ERR_OK = 0;
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				goods: [],
				listHeigth: [],
				scrollY: 0
			}
		},
		computed: {
			currentIndex() {
				for (let i = 0; i<this.listHeigth.length; i++){
					let height1 = this.listHeigth[i];
					let height2 = this.listHeigth[i + 1]
					if(!height2 ||(this.scrollY >= height1 && this.scrollY < height2)){
						return i;
						console.log(i);
					}
				}
				return 0;
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if (response.errno === ERR_OK) {
					this.goods = response.data;
					this.$nextTick(() => {
						this._initScroll();
						this._calculateHeight();
					});
				}
			});
		},
		methods: {
			selectMenu(index, event) {
				if(!event._constructed){
					return;
				}
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let el = foodList[index];
				this.foodScroll.scrollToElement(el,300);
				console.log(index);
			},
			_initScroll() {
				this.menuScroll = new BScroll(this.$refs.menuWrapper, {
					click: true
				});
				this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
					probeType: 3
				});
				this.foodScroll.on('scroll', (pos) => {
					this.scrollY =Math.abs(Math.round(pos.y));
				});
			},
			_calculateHeight(){
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let height = 0;
				this.listHeigth.push(height);
				for (let i = 0; i < foodList.length; i++){
					let item = foodList[i];
					height += item.clientHeight;
					this.listHeigth.push(height);
				}
			}
		}
	};
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl";
	.goods{
		position:absolute;
		top:174px;bottom:46px;
		width:100%;display:flex;
		overflow:hidden;
	}
	.menu-wrapper{
		flex:0 0 80px;
		background:#f3f5f7;
	}
	.foods-wrapper{
		flex:1;
	}
	.menu-item{
		display:table;
		height:54px;
		width:56px;
		line-height:14px;
		padding:0 12px;
		vertical-align:middle;
	}
	.current{
		position:relative;margin-top:-1px;z-index:10;background:#fff;
	}
	.current .text{
		font-weight:700;color:#000;
	}
	.icon{
    display:inline-block;width:12px;height:12px;margin-right:2px;background-size:12px;
    background-repeat:no-repeat;vertical-align: middle;
  }
  .icon .decrease{
    background-image:url(decrease_3@2x.png);
  }
  .icon .discount{
    background-image:url(discount_3@2x.png);
  }
  .icon .guarantee{
    background-image:url(guarantee_3@2x.png);
  }
  .icon .invoice{
    background-image:url(invoice_3@2x.png);
  }
   .foods-wrapper .text{
  	border-1px(rgba(7, 17, 27, 0.1));
  	font-size:12px;display:table-cell;
  	width:56px;vertical-align:middle;
  }
  .border-1px{
  	border-1px(rgba(7, 17, 27, 0.1));
  }
  .foods-wrapper .title{
  	padding-left:14px;height:26px;line-height:26px;border-left:2px solid #d9dde1;
  	font-size:12px;color:rgb(147,153, 159);
  	background:#f3f5f7;
  	 }
  	 .food-item{
  	 	display:flex;padding:18px;
  	 	border-bottom:1px solid #dadada;
  	 }
  	 .food-item:last-child{
  	 	border:none;
  	 	margin-bottom:0;
  	 }
  	 .food-item .icon{
  	 	flex:0 0 57px;
  	 }
  	 .food-item .content .name{
  	 	font-size:14px;
  	 	color:rgb(7, 17, 27);
  	 	line-height:14px;
  	 	margin:0;
  	 }
  	 .food-item .content .extra{
  	 	font-size:10px;
  	 	color:rgb(147,153,159);
  	 	margin:8px 0;
  	 }
  	 .food-item .desc{
  	 	font-size:10px;
  	 	color:rgb(147,153,159);
  	 	margin:5px 0 0 0;
  	 	line-height:16px;
  	 }
  	 .food-item  .price span{
  	 	font-size:14px;
  	 	color:rgb(240,20,20);
  	 	font-weight:700;
  	 	line-height:24px;
  	 }
  	 .food-item .price .oldPrice{
  	 	font-size:12px;
  	 	color:rgb(147,153,159);
  	 	font-weight:normal;
  	 	line-height:24px;
  	 	text-decoration: line-through;
  	 }
</style>