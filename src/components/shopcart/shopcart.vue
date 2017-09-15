<template>
    <div class="shopcart">
        <div class="shopcart-content" @click="flodShow">
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
        <div class="ball-container">
            <transition-group name="drop" v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:after-enter="afterEnter">
                <div class="ball-item" v-for="ball in balls" v-show="ball.show" :key="1">
                    <div class="inner inner-hook" :key="2"></div>
                </div>
            </transition-group>
        </div>
        <transition name="flod">
            <div class="shopcart-list" v-show="showFlod">
                <div class="shopcart-list-header">
                    <span class="left">购物车</span> <span class="right">清空</span>
                </div>
                <div class="shopcart-list-main">
                    <div class="shopcart-list-item" v-for="food in selectFoods">
                        <div class="left">{{food.name}}</div>
                        <div class="right">
                            <div class="price">￥{{food.price * food.count}}</div>
                            <div class="control-wrap">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
  import cartcontrol from '../cartcontrol/cartcontrol.vue'

  export default {
    data () {
      return {
        balls: [{show: false}, {show: false}, {show: false}, {show: false}, {show: false}],
        dropBall: [],
        showFlod: false
      }
    },
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
    methods: {
      drop (el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i]
          if (!ball.show) {
            ball.show = true
            ball.el = el
            this.dropBall.push(ball)
            return
          }
        }
      },
      beforeEnter (el) {
        let count = this.balls.length
        while (count--) {
          let ball = this.balls[count]
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect()
            let x = rect.left - 32
            let y = -(window.innerHeight - rect.top - 22)
            el.style.display = ''
            el.style.webkitTransform = `translate3d(0,${y}px,0)`
            el.style.transform = `translate3d(0,${y}px,0)`
            let inner = el.getElementsByClassName('inner-hook')[0]
            inner.style.webkitTransform = `translate3d(${x}px,0,0)`
            inner.style.transform = `translate3d(${x}px,0,0)`
          }
        }
      },
      enter (el) {
        /* eslint-disable no-unused-vars */
        let rf = el.offsetHeight
        console.log(rf)
        this.$nextTick(() => {
          el.style.display = ''
          el.style.webkitTransform = 'translate3d(0,0,0)'
          el.style.transform = 'translate3d(0,0,0)'
          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = 'translate3d(0,0,0)'
          inner.style.transform = 'translate3d(0,0,0)'
        })
      },
      afterEnter (el) {
        let ball = this.dropBall.shift()
        if (ball) {
          ball.show = false
          el.style.display = 'none'
        }
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
      },
      showFlod () {
        if (this.selectFoods.length < 1) {
          this.showFlod = false
        }
      }
    },
    components: {
      cartcontrol
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin.scss";
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
                margin-top:1px;
                line-height:48px;
                background-color:rgb(43, 51, 59);
                text-align:center;
                font-size:12px;
                font-weight:700;
                color:rgba(255, 255, 255, 0.4);
                &.heighlight{
                    background-color:#00b43c;
                    color:#fff;
                }
            }
        }
        .ball-container{
            .ball-item{
                position:fixed;
                left:32px;
                bottom:22px;
                z-index:200;
                .inner{
                    width:16px;
                    height:16px;
                    border-radius:50%;
                    background:rgb(0, 160, 220);
                }
            }
        }
        .shopcart-list{
            position:fixed;
            left:0;
            bottom:47px;
            max-height:250px;
            width:100%;
            overflow:hidden;
            z-index:-1;
            .shopcart-list-header{
                display:flex;
                justify-content:space-between;
                height:40px;
                line-height:40px;
                padding:0 18px;
                background-color:#f3f5f7;
                border-bottom:2px solid rgba(7, 17, 27, 0.1);
                .left{
                    text-align:left;
                    font-size:14px;
                    font-weight:200;
                    color:rgb(7, 17, 27);
                }
                .right{
                    font-size:12px;
                    color:rgb(0, 160, 220);
                }
            }
            .shopcart-list-main{
                max-height:210px;
                overflow:hidden;
                background-color:#fff;
                .shopcart-list-item{
                    display:flex;
                    justify-content:space-between;
                    font-size:0;
                    height:48px;
                    margin:0 18px;
                    box-sizing:border-box;
                    line-height:48px;
                    @include border-1px(rgba(7, 17, 27, 0.1));
                    &:last-child{
                        @include border-none()
                    }
                    .left{
                        font-size:14px;
                        color:rgb(7, 17, 27);
                    }
                    .right{
                        .price{
                            display:inline-block;
                            vertical-align:top;
                            margin-right:6px;
                            font-size:14px;
                            font-weight:700;
                            color:rgb(240, 20, 20);
                        }
                        .control-wrap{
                            vertical-align:top;
                            display:inline-block;
                            height:100%;
                            box-sizing:border-box;
                            margin-top:3px;
                        }
                    }
                }
            }
        }
    }
    .drop-enter-active{
        transition:all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.21);
        .inner{
            transition:all 0.4s linear;
        }
    }
</style>