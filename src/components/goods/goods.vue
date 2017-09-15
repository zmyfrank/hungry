<template>
    <div class="goods">
        <div class="goods-menu-wrapper" ref="menuWrapper">
            <ul>
                <li class="goods-menu-item" v-for="(item, index) in goods" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
                    <div class="text">
                        <icon class="icon" v-show="item.type>-1" :type="item.type" :num="3" :size="12"></icon>
                        <span class="text-item">{{item.name}}</span>
                    </div>
                </li>
            </ul>
        </div>
        <div class="goods-item-wrapper" ref="itemWrapper">
            <ul>
                <li class="goods-item goods-item-hook" v-for="item in goods">
                    <h2 class="title">{{item.name}}</h2>
                    <div class="item" v-for="food in item.foods">
                        <div class="item-img">
                            <img :src="food.icon" width="57" height="57" alt="商品图片">
                        </div>
                        <div class="item-description">
                            <h2 class="item-name">{{food.name}}</h2>
                            <p class="item-des" v-show="food.description&&food.description!==''">{{food.description}}</p>
                            <div class="item-count">
                                <span class="item-count-left">月售{{food.sellCount}}份</span><span v-show="food.rating&&food.rating!==''" class="item-count-right">好评率{{food.rating}}%</span>
                            </div>
                            <div class="item-price">
                                <span class="item-price-left">¥{{food.price}}</span><span v-show="food.oldPrice&&food.oldPrice!==''" class="item-price-right">¥{{food.oldPrice}}</span>
                            </div>
                        </div>
                        <div class="cart-wrapper">
                            <cartcontrol :food="food"></cartcontrol>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
        <shopCart ref="shopCart" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :selectFoods="selectFoods"></shopCart>
    </div>
</template>

<script>
  import icon from '../icon/icon.vue'
  import BScroll from 'better-scroll'
  import shopCart from '../shopcart/shopcart.vue'
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
  import Bus from '../../bus'

  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        itemHeight: [],
        classArr: [],
        scrollY: 0
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.itemHeight.length; i++) {
          let heightFirst = this.itemHeight[i]
          let heightNext = this.itemHeight[i + 1]
          if (!heightNext || (this.scrollY >= heightFirst && this.scrollY < heightNext)) {
            // console.log(`当前高度${this.scrollY}，第一高度${heightFirst}，当前的i值${i}`)
            // console.log(this.itemHeight)
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let seLectFood = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              seLectFood.push(food)
            }
          })
        })
        return seLectFood
      }
    },
    created () {
      this.$http.get('api/goods')
        .then(res => {
          let resData = res.body
          if (resData.errno === ERR_OK) {
            this.goods = resData.data
            this.$nextTick(() => {
              // 应用better scroll
              this._initScroll()
              // 计算高度
              this._calculateHeight()
            })
          }
        })
      Bus.$on('car.add', (target) => {
        this._dorp(target)
      })
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {click: true})
        this.itemScroll = new BScroll(this.$refs.itemWrapper, {probeType: 3, click: true})
        this.itemScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight () {
        this.classArr = this.$refs.itemWrapper.getElementsByClassName('goods-item-hook')
        let height = 0
        this.itemHeight.push(height)
        for (let i = 0; i < this.classArr.length; i++) {
          height += this.classArr[i].clientHeight
          this.itemHeight.push(height)
        }
      },
      selectMenu (index) {
        let el = this.classArr[index]
        this.itemScroll.scrollToElement(el, 300)
      },
      _dorp (target) {
        // 优化体验
        this.$nextTick(() => this.$refs.shopCart.drop(target))
      }
    },
    components: {
      icon,
      shopCart,
      cartcontrol
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin";
    .goods{
        display:flex;
        position:absolute;
        width:100%;
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
                padding:0 12px;
                font-size:0;
                width:56px;
                height:54px;
                line-height:14px;
                background-color:#f3f5f7;
                &.current{
                    position:relative;
                    z-index:10;
                    margin-top:-1px;
                    background-color:#fff;
                    font-weight:700;
                    .text{
                        @include border-none()
                    }
                }
                .icon{
                    margin-right:2px;
                    line-height:24px;
                }
                .text{
                    display:table-cell;
                    vertical-align:middle;
                    @include border-1px(rgba(7, 17, 27, 0.1));
                    .text-item{
                        vertical-align:top;
                        font-size:12px;
                    }
                }
            }
        }
        .goods-item-wrapper{
            flex:1;
            .goods-item{
                .title{
                    height:26px;
                    padding-left:28px;
                    font-size:12px;
                    line-height:26px;
                    color:rgb(147, 153, 159);
                    background-color:#f3f5f7;
                    border-left:2px solid #d9dde1;
                }
                .item{
                    display:flex;
                    position:relative;
                    margin:18px 18px 0 18px;
                    padding-bottom:18px;
                    @include border-1px(rgba(7, 17, 27, 0.1));
                    &:last-child{
                        @include border-none()
                    }
                    .cart-wrapper{
                        position:absolute;
                        right:0;
                        bottom:12px;
                    }
                    .item-img{
                        flex:0 0 57px;
                        margin-right:10px;
                    }
                    .item-description{
                        padding-top:2px;
                        .item-name{
                            font-size:14px;
                            line-height:14px;
                            color:rgb(7, 17, 27);
                        }
                        .item-des{
                            margin-top:8px;
                            font-size:10px;
                            line-height:12px;
                            color:rgb(14, 153, 159);
                        }
                        .item-count{
                            margin-top:8px;
                            font-size:10px;
                            line-height:10px;
                            color:rgb(14, 153, 159);
                            .item-count-right{
                                margin-left:12px;
                            }
                        }
                        .item-price{
                            font-weight:700;
                            line-height:24px;
                            .item-price-left{
                                margin-right:8px;
                                font-size:14px;
                                color:rgb(240, 20, 20);
                            }
                            .item-price-right{
                                margin-bottom:2px;
                                font-size:10px;
                                text-decoration:line-through;
                                color:rgb(147, 153, 159);
                            }
                        }
                    }
                }
            }
        }
    }
</style>