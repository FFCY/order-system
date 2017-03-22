<template>
  <div class="header">
   <div class="content-wrapper">
     <div class="avatar">
       <img width="64" height="64" :src="seller.avatar">
     </div>
     <div class="content">
       <div class="title">
         <span class="brand"></span>
         <span class="name">{{seller.name}}</span>
       </div>
       <div class="description">
         {{seller.description}}/{{seller.deliveryTime}}分钟送达
       </div>
       <div v-if="seller.supports" class="support">
         <span class="icon" :class="classMap[seller.supports[0].type]"></span>
         <span class="text">{{seller.supports[0].description}}</span>
       </div>
     </div>
     <div v-if="seller.supports" class="support-count" @click="showDetail">
       <span class="count">{{seller.supports.length}}个</span>
       <span class="icon-keyboard_arrow_right"></span>
     </div>
   </div>
    <div class="bulletin-wrapper">
      <span class="bulltein-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
     <transition name="fade" type="transition" >
        <div v-show="detailShow" class="detail">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <div class="name">{{seller.name}}</div>
          <div class="star-wrapper">
          <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
           <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="descript">{{seller.bulletin}}</div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
     </transition>
  </div>
</template>
<script type="text/ecmascript-6">
import star from 'components/star/star';
  export default {
      props: {
          seller: {
              type: Object
          }
      },
      data() {
        return {
          detailShow: false
        };
      },
      methods: {
        showDetail() {
          this.detailShow = true;
        },
        hideDetail() {
          this.detailShow = false;
        }
      },
    created() {
          this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .header{overflow:hidden; color:#fff;position:relative;background:rgba(7, 17, 27, 0.5);}
  .content-wrapper{padding:24px 12px 18px 24px;overflow:hidden; position:relative;}
  .avatar{float:left;}
  .avatar img{border-radius:2px;}
  .content{float:left;font-size:14px;margin-left:16px;}
  .title{margin:1px 8px 8px 2px;overflow:hidden;}
  .brand{width:30px;height:18px;display: inline-block;background-image:url(../../assets/brand@2x.png);
  background-size:30px 18px;background-repeat:no-repeat;
  }
  .name{
    margin-left:6px;
    font-size:16px;
    line-height:18px;
    font-weight:bold;vertical-align: top;
  }
  .description{
    margin-bottom:10px;
    line-height:12px;
    font-size:12px;
  }
  .icon{
    display:inline-block;width:12px;height:12px;margin-right:4px;background-size:12px;
    background-repeat:no-repeat;vertical-align: middle;
  }
  .icon .decrease{
    background-image:url(../../assets/decrease_1@2x.png);
  }
  .icon .discount{
    background-image:url(../../assets/discount_1@2x.png);
  }
  .icon .guarantee{
    background-image:url(../../assets/guarantee_1@2x.png);
  }
  .support .icon{
    display:inline-block;width:12px;height:12px;margin-left:4px;
    background-size:12px 12px;background-repeat:no-repeat;
  }
  .decrease{
    background-image:url(../../assets/decrease_1@2x.png);
  }
  .discount{
    background-image:url(../../assets/discount_1@2x.png);
  }
  .guarantee{
    background-image:url(../../assets/guarantee_1@2x.png);
  }
  .invoice{
    background-image:url(../../assets/invoice_1@2x.png);
  }
  .special{
    background-image:url(../../assets/special_1@2x.png);
  }
  .text{
    line-height:12px;
    font-size:10px;
  }
  .icon-check_circle:before{
    content: "\e905";
  }
  .icon-close:before{
    content: "\e906";
  }
  .icon-remove_circle_outline:before{
    content: "\e907";
  }
  .icon-keyboard_arrow_right:before{
    content: "\e909";
  }
  .support-count{
    position: absolute;
    right:12px;bottom:18px;padding:0 8px;height:23px;line-height:24px;border-radius:14px;
    background:rgba(0, 0, 0, 0.2);
    text-align:center;
  }
  .count{
    font-size:10px;vertical-align:top;
  }
  .icon-keyboard_arrow_right{
    font-size:10px;line-height:20px;
  }
  .bulletin-wrapper {
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    position:relative;background:rgba(0, 0, 0, 0.7);
  }
  .bulletin-wrapper .bulltein-title{
    display:inline-block;
    width:22px;height:12px; background-image:url(../../assets/bulletin@2x.png);
    background-size:22px 12px;background-repeat:no-repeat;
    vertical-align:middle;
  }
  .bulletin-wrapper .bulletin-text{
    font-size:10px;margin:0 4px;
  }
  .bulletin-wrapper .icon-keyboard_arrow_right{
    position:absolute;right:12px;top:8px;font-size:12px;
  }
  .header .background{
    position:absolute;left:0;top:0;z-index:-1;width:100%;height:100%;filter:blur(10px);
  }
  .header .detail{
    position:fixed;
    left:0;top:0;
    z-index:100;
    width:100%;height:100%;overflow:auto;
    background: rgba(7, 17, 27, 0.8);
  }
  .fade-enter-active, .fade-leave-active{
    transition: all .9s ease;
  }
  .fade-enter, .fade-leave-active {
     opacity: 0
    background: rgba(7, 17, 27, 0)
}
  .clearfix{
    display:inline-block;
  }
  .clearfix:after{
    display:block;
    content:'';height:0;line-height:0;clear:both;visibility:hiden;
  }
  .detail-wrapper{
    min-height:100%;
    width:100%;
  }
  .detail-wrapper .detail-main{
    margin-top:8px;padding-bottom:64px;
  }
  .detail-close{
    position:relative;width:32px;height:32px;
    margin:-64px auto 0 auto;clear:both;font-size:32px;
  }
  .detail-main .name{
    line-height:16px;text-align:center;font-size:16px;
    font-weight:700;width:100%;margin-top:20px;
  }
  .star-wrapper{
    margin-top:18px;
    padding:2px 0;
    text-align:center;
  }
 .detail-main .title{
    display:flex;
    width:80%;margin:35px auto 24px auto;
  }
  .detail-main .title .line{
    flex:1;position:relative;
    top:-6px;border-bottom:1px solid rgba(255, 255, 255, 0.2);
  }
  .detail-main .title .text{
    padding:0 12px;
    font-size:14px;
    line-height:14px;
    font-weight:700;
  }
  .bulletin{
    width:100%;margin:0 auto;
  }
  .supports{
    padding:0 12px;
  }
  .supports .support-item{
    margin:4px 0;
  }
  .bulletin .content{
    padding:0 12px;line-height:24px;font-size:12px;
  }
  .descript{
    padding:0 12px;font-size:12px;line-height:16px;color:#eaeaea;
  }
</style>
