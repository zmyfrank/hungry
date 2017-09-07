<template>
    <div class="icon" :class="[icon,iconSize]"></div>
</template>

<script>
  export default {
    props: {
      type: {
        type: Number
      },
      num: {
        type: Number
      },
      size: {
        type: Number
      }
    },
    data () {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      }
    },
    computed: {
      icon () {
        return this.classMap[this.type] + '-' + this.num
      },
      iconSize () {
        return 'icon-' + this.size
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin";
    .icon{
        display:inline-block;
        vertical-align:top;
        background-repeat:no-repeat;
        &.icon-12{
            width:12px;
            height:12px;
            background-size:12px 12px;
        }
        &.icon-16 {
            width:16px;
            height:16px;
            background-size:16px 16px;
        }
        &.decrease-1{
            @include bg-img('decrease_1')
        }
        &.discount-1{
            @include bg-img('discount_1')
        }
        &.guarantee-1{
            @include bg-img('guarantee_1')
        }
        &.invoice-1{
            @include bg-img('invoice_1')
        }
        &.special-1{
            @include bg-img('special_1')
        }
        &.decrease-2{
            @include bg-img('decrease_2')
        }
        &.discount-2{
            @include bg-img('discount_2')
        }
        &.guarantee-2{
            @include bg-img('guarantee_2')
        }
        &.invoice-2{
            @include bg-img('invoice_2')
        }
        &.special-2{
            @include bg-img('special_2')
        }
        &.decrease-3{
            @include bg-img('decrease_3')
        }
        &.discount-3{
            @include bg-img('discount_3')
        }
        &.guarantee-3{
            @include bg-img('guarantee_3')
        }
        &.invoice-3{
            @include bg-img('invoice_3')
        }
        &.special-3{
            @include bg-img('special_3')
        }
        &.decrease-4{
            @include bg-img('decrease_4')
        }
        &.discount-4{
            @include bg-img('discount_4')
        }
        &.guarantee-4{
            @include bg-img('guarantee_4')
        }
        &.invoice-4{
            @include bg-img('invoice_4')
        }
        &.special-4{
            @include bg-img('special_4')
        }
    }
</style>