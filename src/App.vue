<template>
  <div>
    <tou :seller="seller">

    </tou>
  <div class="header">
   <!--  I am a  header -->
  </div>
  <div class="tab">
    <div class="tab-item">
      <router-link to="/goods">商品</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/ratings">评论</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/seller">评论</router-link>
    </div>
  </div>
    <router-view>

    </router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header.vue';
  const ERR_OK = 0;
  export default {
      data() {
          return {
              seller: {}
          };
      },
    created() {
          this.$http.get('/api/seller').then((response) => {
              response = response.body;
              if (response.errno === ERR_OK) {
                  this.seller = response.data;
              }
          });
    },
      components: {
          tou: header
      }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  .tab{display:flex;width:100%;height:40px;line-height:40px;}
  .tab-item{flex:1;text-align:center;border-1px(rgba(7, 17, 27, 0.1));}
  a{display:block;text-decration:none;font-size:14px;color:#778593;}
  .active{color:#cb3332;}
</style>
