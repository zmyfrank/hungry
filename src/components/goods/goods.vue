<template>
    <div class="goods">
        <div class="goods-menu-wrapper">
            <div class="goods-menu-item" v-for="item in goods">
                <span class="text">
                     <icon class="icon" v-show="item.type>-1" :type="item.type" :num="3" :size="12"></icon>
                    <span class="text-item">{{item.name}}</span>
                </span>
            </div>
        </div>
        <div class="goods-item-wrapper"></div>
    </div>
</template>

<script>
  import icon from '../icon/icon.vue'

  const ERR_OK = 0
  export default {
    data () {
      return {
        goods: {}
      }
    },
    created () {
      this.$http.get('api/goods')
        .then(res => {
          let resData = res.body
          if (resData.errno === ERR_OK) {
            this.goods = resData.data
            console.log(this.goods)
          }
        })
    },
    components: {
      icon
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin";
    .goods{
        display:flex;
        position:absolute;
        top:174px;
        bottom:46px;
        overflow:hidden;
        .goods-menu-wrapper{
            flex:0 0 80px;
            width:80px;
            background-color:#f3f5f7;
            font-size:12px;
            color:rgb(240, 20, 20);
            .goods-menu-item{
                display:table;
                margin:0 12px;
                font-size:0;
                width:56px;
                height:54px;
                line-height:14px;
                @include border-1px(rgba(7, 17, 27, 0.1));
                .icon {
                    margin-right:2px;
                    line-height:24px;
                }
                .text{
                    display:table-cell;
                    vertical-align:middle;
                    .text-item {
                        vertical-align:top;
                        font-size:12px;
                    }
                }
            }
        }
        .goods-item-wrapper{
            flex:1;
        }
    }
</style>