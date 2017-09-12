<template>
    <div class="shopcart">
        <div class="shopcart-content">
            <div class="content-left">
                <div class="car-wrapper">
                    <div class="car-item" :class="{'highlight':totalCount>0}">
                        <i class="icon-shopping_cart"></i>
                    </div>
                    <div v-show="totalCount>0" class="num">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight':totalCount>0}">
                    ￥{{totalPrice}}
                </div>
                <div class="desc">
                    另需配送费￥{{deliveryPrice}}元
                </div>
            </div>
            <div class="content-right" :class="{'heighlight':payDesc==='去结算'}">
                <span class="text">{{payDesc}}</span>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    props: {
      selectFoods: {
        type: Array,
        default () {
          return []
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((foods) => {
          total += (foods.count * foods.price)
        })
        return total
      },
      totalCount () {
        let total = 0
        this.selectFoods.forEach((foods) => {
          total += foods.count
        })
        return total
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice
          return `还差￥${diff}元起送`
        } else if (this.totalPrice >= this.minPrice) {
          return '去结算'
        }
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    .shopcart{
        position:fixed;
        left:0;
        bottom:0;
        width:100%;
        height:48px;
        background-color:#141d27;
        .shopcart-content{
            display:flex;
            .content-left{
                flex:1;
                font-size:0;
                .car-wrapper{
                    display:inline-block;
                    position:relative;
                    top:-10px;
                    padding:6px;
                    margin:0 12px;
                    box-sizing:border-box;
                    vertical-align:top;
                    border-radius:50%;
                    height:56px;
                    width:56px;
                    background-color:#141d27;
                    color:rgba(255, 255, 255, 0.4);
                    .car-item{
                        width:100%;
                        height:100%;
                        border-radius:50%;
                        text-align:center;
                        background-color:#2b343c;
                        &.highlight{
                            background-color:rgb(0, 160, 220);
                            .icon-shopping_cart{
                                color:#fff;
                            }
                        }
                        .icon-shopping_cart{
                            font-size:24px;
                            line-height:44px;
                        }
                    }
                    .num{
                        position:absolute;
                        top:0;
                        right:0;
                        width:24px;
                        height:16px;
                        padding:0 6px;
                        box-sizing:border-box;
                        background-color:rgb(240, 20, 20);
                        box-shadow:0 4px 8px 0 rgba(0, 0, 0, 0.4);
                        border-radius:6px;
                        font-size:9px;
                        font-weight:700;
                        line-height:16px;
                        text-align:center;
                        color:#fff;
                    }
                }
                .price{
                    display:inline-block;
                    padding-right:12px;
                    margin:12px 0;
                    font-size:16px;
                    font-weight:700;
                    line-height:24px;
                    border-right:1px solid rgba(255, 255, 255, 0.1);
                    &.highlight{
                        color:#fff;
                    }
                }
                .desc{
                    display:inline-block;
                    margin:12px;
                    font-size:10px;
                    line-height:24px;
                }
            }
            .content-right{
                flex:0 0 105px;
                width:105px;
                height:48px;
                line-height:48px;
                background-color:rgb(43, 51, 59);
                text-align:center;
                font-size:12px;
                font-weight:700;
                color:rgba(255, 255, 255, 0.4);
                &.heighlight{
                    background-color:#00b43c;
                    color: #fff;
                }
            }
        }
    }
</style>