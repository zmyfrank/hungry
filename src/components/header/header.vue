<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img width="64" height="64" :src="seller.avatar" alt="商家头像">
            </div>
            <div class="content">
                <div class="content-title">
                    <span class="content-title-brand"></span> <span class="content-title-name">{{seller.name}}</span>
                </div>
                <div class="content-description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div class="content-support" v-if="seller.supports">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span> <span class="content-support-text">
                        {{seller.supports[0].description}}
                    </span>
                </div>
            </div>
            <div class="support-content" v-if="seller.supports" @click="detailShow">
                <span class="count">{{seller.supports.length}}个</span> <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="detailShow">
            <span class="bulletin-wrapper-brand"></span><span class="bulletin-wrapper-main">{{seller.bulletin}}</span> <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background-img">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>
        <transition name="fade">
            <div class="detail" v-show="showDetail">
                <div class="detail-wrapper clearfix">
                    <div class="detail-main">
                        <div class="detail-main-header">
                            {{seller.name}}
                        </div>
                        <div class="detail-main-star">
                            <star :size="48" :score="seller.score"></star>
                        </div>
                        <div class="detail-main-title">
                            <div class="title-line"></div>
                            <div class="title-main">优惠信息</div>
                            <div class="title-line"></div>
                        </div>
                        <div class="detail-main-support-wrapper">
                            <div class="detail-main-support-item" v-for="item in seller.supports">
                                <span class="item-icon" :class="classMap[item.type]"></span> <span class="item-text">{{item.description}}</span>
                            </div>
                        </div>
                        <div class="detail-main-title">
                            <div class="title-line"></div>
                            <div class="title-main">商家公告</div>
                            <div class="title-line"></div>
                        </div>
                        <div class="detail-main-bulletin">
                            <div class="text">{{seller.bulletin}}</div>
                        </div>
                    </div>
                </div>
                <div class="detail-footer" @click="detailShow">
                    <i class="icon-close"></i>
                </div>
            </div>
        </transition>

    </div>
</template>

<script>
  import star from '../star/star.vue'

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        showDetail: false
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
    },
    methods: {
      detailShow () {
        this.showDetail = !this.showDetail
      }
    },
    computed: {},
    components: {
      star
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../common/scss/mixin";
    .header{
        position:relative;
        color:#fff;
        background-color:rgba(7, 17, 27, 0.5);
        .content-wrapper{
            padding:24px 12px 18px 24px;
            position:relative;
            vertical-align:top;
            font-size:0;
            .avatar{
                display:inline-block;
            }
            .content{
                display:inline-block;
                margin-left:16px;
                vertical-align:top;
                font-size:12px;
                line-height:12px;
                .content-title{
                    margin-bottom:8px;
                    .content-title-brand{
                        display:inline-block;
                        margin-right:6px;
                        vertical-align:top;
                        width:30px;
                        height:18px;
                        @include bg-img('brand');
                        background-size:30px 18px;
                        background-repeat:no-repeat;
                    }
                    .content-title-name{
                        color:#fff;
                        line-height:18px;
                        font-size:16px;
                        font-weight:bold;
                    }
                }
                .content-description{
                    margin-bottom:10px;
                }
                .content-support{
                    .icon{
                        display:inline-block;
                        margin-right:4px;
                        vertical-align:top;
                        width:12px;
                        height:12px;
                        background-size:12px;
                        &.decrease{
                            @include bg-img('decrease_1')
                        }
                        &.discount{
                            @include bg-img('discount_1')
                        }
                        &.guarantee{
                            @include bg-img('guarantee_1')
                        }
                        &.invoice{
                            @include bg-img('invoice_1')
                        }
                        &.special{
                            @include bg-img('special_1')
                        }
                    }
                    .text{
                        font-size:10px;
                    }
                }
            }
            .support-content{
                position:absolute;
                right:12px;
                bottom:14px;
                padding:0 8px;
                height:24px;
                text-align:center;
                background-color:rgba(0, 0, 0, 0.2);
                font-size:10px;
                line-height:24px;
                border-radius:14px;
                .count{
                    vertical-align:top;
                }
                .icon-keyboard_arrow_right{
                    margin-left:2px;
                    line-height:24px;
                }
            }
        }
        .bulletin-wrapper{
            position:relative;
            padding:0 22px 0 12px;
            height:28px;
            line-height:28px;
            background-color:rgba(7, 17, 27, 0.2);
            text-overflow:ellipsis;
            white-space:nowrap;
            overflow:hidden;
            .bulletin-wrapper-brand{
                display:inline-block;
                vertical-align:top;
                margin-top:8px;
                width:22px;
                height:12px;
                @include bg-img('bulletin');
                background-size:22px 12px;
                background-repeat:no-repeat;
            }
            .bulletin-wrapper-main{
                vertical-align:top;
                margin:0 4px;
                font-size:10px;
            }
            .icon-keyboard_arrow_right{
                position:absolute;
                font-size:10px;
                top:8px;
                right:12px;
            }
        }
        .background-img{
            position:absolute;
            top:0;
            left:0;
            z-index:-1;
            width:100%;
            height:100%;
            filter:blur(10px);
        }
        .detail{
            position:fixed;
            top:0;
            left:0;
            width:100%;
            height:100%;
            overflow:auto;
            z-index:100;
            transition:all 0.5s;
            opacity:1;
            background-color:rgba(7, 17, 27, 0.8);
            backdrop-filter:blur(10px);
            &.fade-enter, &.fade-leave-to{
                opacity:0;
                background-color:rgba(7, 17, 27, 0);
            }
            .detail-wrapper{
                min-height:100%;
                width:100%;
                .detail-main{
                    margin-top:64px;
                    padding-bottom:64px;
                    .detail-main-header{
                        font-size:16px;
                        font-weight:700;
                        line-height:16px;
                        text-align:center;
                    }
                    .detail-main-star{
                        margin-top:32px;
                        text-align:center;
                    }
                    .detail-main-title{
                        display:flex;
                        width:80%;
                        margin:28px auto 24px auto;
                        .title-line{
                            margin-bottom:6px;
                            flex:1;
                            border-bottom:1px solid rgba(255, 255, 255, 0.2);
                        }
                        .title-main{
                            padding:0 12px;
                            font-size:14px;
                            line-height:14px;
                            font-weight:700;
                        }
                    }
                    .detail-main-support-wrapper{
                        width:80%;
                        margin:0 auto;
                        .detail-main-support-item{
                            margin-bottom:12px;
                            padding:0 12px;
                            &:last-child{
                                margin-bottom:0;
                            }
                            .item-icon{
                                display:inline-block;
                                margin-right:6px;
                                vertical-align:top;
                                width:16px;
                                height:16px;
                                background-size:16px 16px;
                                background-repeat:no-repeat;
                                &.decrease{
                                    @include bg-img('decrease_1')
                                }
                                &.discount{
                                    @include bg-img('discount_1')
                                }
                                &.guarantee{
                                    @include bg-img('guarantee_1')
                                }
                                &.invoice{
                                    @include bg-img('invoice_1')
                                }
                                &.special{
                                    @include bg-img('special_1')
                                }
                            }
                            .item-text{
                                font-size:12px;
                                line-height:12px;
                            }
                        }
                    }
                    .detail-main-bulletin{
                        width:80%;
                        margin:0 auto;
                        .text{
                            padding:0 12px;
                            font-size:12px;
                            line-height:24px;
                        }
                    }
                }
            }
            .detail-footer{
                position:relative;
                width:32px;
                height:32px;
                margin:-64px auto 0 auto;
                clear:both;
                font-size:32px;
            }
        }
    }
</style>`