<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease" v-show="food.count>0" @click="decreaseItem" :key="1">
                <span class="inner icon-remove_circle_outline"></span>
            </div>
        </transition>
            <div class="cart-count" v-show="food.count>0" :key="2">{{food.count}}</div>
            <div class="cart-add icon-add_circle" @click="addItem" :key="3"></div>
    </div>
</template>

<script>
  import Vue from 'vue'

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addItem () {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      decreaseItem () {
        this.food.count--
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    .cartcontrol{
        display:inline-block;
        font-size:0;
        .cart-decrease{
            display:inline-block;
            padding:6px;
            transition:all 0.4s linear;
            .inner{
                display:inline-block;
                font-size:24px;
                line-height:24px;
                color:rgb(0, 160, 220);
                transition:all 0.4s linear;
                transform:rotate(0);
            }
            &.move-enter-to{
                opacity:1;
                transform:translate3D(0, 0, 0);
            }
            &.move-enter, &.move-leave-to{
                opacity:0;
                transform:translate3D(24px, 0, 0);
                .inner{
                    transform:rotate(180deg);
                }
            }
        }
        .cart-count{
            display:inline-block;
            vertical-align:top;
            padding-top:6px;
            width:12px;
            font-size:10px;
            line-height:24px;
            color:rgb(147, 153, 159);
            text-align:center;
        }
        .cart-add{
            display:inline-block;
            padding:6px;
            font-size:24px;
            line-height:24px;
            color:rgb(0, 160, 220);
        }
    }
</style>