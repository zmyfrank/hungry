<template>
    <div class="star" :class="starType">
        <span v-for="itemClass in itemClasses " class="star-item" :class="itemClass" track-by="$index"></span>
    </div>
</template>

<script>
  const LENGTH = 5
  const CLS_ON = 'on'
  const CLS_HALF = 'half'
  const CLS_OFF = 'off'
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType () {
        return 'star-' + this.size
      },
      itemClasses () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        let hasDecimal = score % 1 !== 0
        for (let i = 0; i < Math.floor(score); i++) {
          result.push(CLS_ON)
        }
        if (hasDecimal) {
          result.push(CLS_HALF)
        }
        while (result.length < LENGTH) {
          result.push(CLS_OFF)
        }
        return result
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin";
    .star{
        font-size:0;
        .star-item{
            display:inline-block;
            background-repeat:no-repeat;
        }
        &.star-48{
            .star-item{
                &:nth-child(n+2){
                    margin-left:22px;
                }
                width:20px;
                height:20px;
                background-size:20px 20px;
                &.on{
                    @include bg-img('star48_on')
                }
                &.half{
                    @include bg-img('star48_half')
                }
                &.off{
                    @include bg-img('star48_off')
                }
            }
        }
        &.star-36{
            .star-item{
                &:nth-child(n+2){
                    margin-left:6px;
                }
                width:15px;
                height:15px;
                background-size:15px 15px;
                &.on{
                    @include bg-img('star36_on')
                }
                &.half{
                    @include bg-img('star36_half')
                }
                &.off{
                    @include bg-img('star36_off')
                }
            }
        }
        &.star-24{
            .star-item{
                &:nth-child(n+2){
                    margin-left:3px;
                }
                width:10px;
                height:10px;
                background-size:10px 10px;
                &.on{
                    @include bg-img('star24_on')
                }
                &.half{
                    @include bg-img('star24_half')
                }
                &.off{
                    @include bg-img('star24_off')
                }
            }
        }
    }
</style>