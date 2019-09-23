<template>
  <div class="v-header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="headerSeller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{headerSeller.name}}</span>
        </div>
        <div class="description">
          {{headerSeller.description}}/{{headerSeller.deliveryTime}}分钟送达
        </div>
        <div v-if="headerSeller.supports" class="support">
          <span class="icon" :class="classMap[headerSeller.supports[0].type]"></span><span class="text">{{headerSeller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="headerSeller.supports" @click="showDetail" class="support-count">
        <span class="count">{{headerSeller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div @click="showDetail" class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{headerSeller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="headerSeller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-content">
            <h1 class="name">{{headerSeller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="headerSeller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="headerSeller.supports" class="supports">
              <li class="supports-item" v-for="(item,index) in headerSeller.supports">
                <span class="icon" :class="classMap[headerSeller.supports[index].type]"></span>
                <span class="text">{{headerSeller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div v-if="headerSeller.bulletin" class="bulletin">
              <p class="content">{{headerSeller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div @click="closeDetail" class="detail-close">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star'

  export default {
    components: {
      star
    },
    props: {
      headerSeller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      }
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      closeDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  };
</script>

<style lang="stylus">
  @import "../../common/stylus/mixin.styl"

  .v-header {
    position: relative;
    overflow: hidden;
    color: #fff;
    background-color: rgba(7, 17, 27, 0.5);
  }

  .v-header .content-wrapper {
    position: relative;
    padding: 24px 12px 18px 24px;
    font-size: 0;
  }

  .v-header .content-wrapper .avatar {
    display: inline-block;
    vertical-align: top;
    border-radius: 2px;

  }

  .v-header .content-wrapper .content {
    display: inline-block;
    margin-left: 16px;
  }

  .v-header .content-wrapper .content .title {
    margin: 2px 0 8px 0;
  }

  .v-header .content-wrapper .content .title .brand {
    display: inline-block;
    width: 30px;
    height: 18px;
    vertical-align: top;
    bg-image("brand");
    background-size: 30px 18px;
    background-repeat: no-repeat;
  }

  .v-header .content-wrapper .content .title .name {
    margin-left: 6px;
    font-size: 16px;
    font-weight: bold;
    line-height: 18px;
  }

  .v-header .content-wrapper .content .description {
    margin-bottom: 10px;
    line-height: 12px;
    font-size: 12px;
    font-weight: 200;
  }

  .v-header .content-wrapper .content .support {
    margin: 10px 0 2px 0;
    font-size: 10px;
  }

  .v-header .content-wrapper .content .support .icon {
    display: inline-block;
    width: 12px;
    height: 12px;

    margin-right: 4px;
    vertical-align: middle;
    background-size: 12px 12px;

    background-repeat: no-repeat;
  }

    &.discount {
    background :url(discount_1@2x.png)
  }

    &.decrease {
    background:url(decrease_1@2x.png);
  }

    &.guarantee {
    background:url(guarantee_1@2x.png);
  }

    &.invoice {
    background:url(invoice_1@2x.png);
  }

    &.special {
    background:url(special_1@2x.png);
  }

  .v-header .content-wrapper .content .support .text {
    line-height: 12px;
  }

  .v-header .content-wrapper .support-count {
    position: absolute;
    right: 12px;
    bottom: 14px;
    padding: 0 8px;
    height: 24px;
    border-radius: 14px;
    background: rgba(0, 0, 0, 0.2);
    text-align: center;
  }

  .v-header .content-wrapper .support-count .count {
    vertical-align: top;
    font-size: 10px;
    line-height: 24px;
  }

  .v-header .content-wrapper .support-count .icon-keyboard_arrow_right {
    margin-left: 2px;
    font-size: 10px;
    line-height: 24px;
  }

  .v-header .bulletin-wrapper {
    position: relative;
    height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background: rgba(7, 17, 27, 0.2);
  }

  .v-header .bulletin-wrapper .bulletin-title {
    display: inline-block;
    margin-right: 4px;
    width: 22px;
    height: 12px;
    vertical-align: middle;
    background-size: 22px 12px;
    background-repeat: no-repeat;
    bg-image('bulletin');
  }

  .v-header .bulletin-wrapper .bulletin-text {
    font-size: 10px;
    line-height: 28px;
  }

  .v-header .bulletin-wrapper .icon-keyboard_arrow_right {
    position: absolute;
    font-size: 10px;
    right: 12px;
    top: 10px;
  }

  .v-header .background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
  }

  .v-header .detail {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 100;
    overflow: auto;
    background: rgba(7, 17, 27, 0.8);
    transition: all 0.5s;

    // &.fade-enter-active, &.fade-leave-active;
    //   opacity: 1;
    //   background: rgba(7,17,27,0.8);
    .v-header .detail &.fade-enter,
    &.fade-leave-active {
      opacity: 0;
      background: rgba(7, 17, 27, 0);
    }
  }

  .v-header .detail .detail-wrapper {
    min-height: 100%;
    width: 100%;
  }

  .v-header .detail .detail-wrapper .detail-content {
    margin-top: 64px;
    padding-bottom: 64px;
  }

  .v-header .detail .detail-wrapper .detail-content .name {
    font-size: 16px;
    line-height: 16px;
    font-weight: 700;
    text-align: center;
  }

  .v-header .detail .detail-wrapper .detail-content .star-wrapper {
    margin-top: 18px;
    padding: 2px 0;
    text-align: center;
  }

  .v-header .detail .detail-wrapper .detail-content .title {
    display: flex;
    width: 80%;
    margin: 28px auto 24px auto;
  }

  .v-header .detail .detail-wrapper .detail-content .title .line {
    flex: 1;
    position: relative;
    top: -6px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }

  .v-header .detail .detail-wrapper .detail-content .title .text {
    padding: 0 12px;
    font-size: 14px;
    font-weight: 700;
  }

  .v-header .detail .detail-wrapper .detail-content .supports {
    margin: 0 auto;
    width: 80%;
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item {
    padding: 0 12px;
    margin-bottom: 12px;
    font-size: 0;
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item &:last-child {
    margin-bottom: 0;
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 16px;
    vertical-align: top;
    background-size: 16px 16px;
    backgroun-repeat: no-repeat;
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon &.decrease {
    bg-image('decrease_2');
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon &.discount {
    bg-image('discount_2');
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon &.guarantee {
    bg-image('guarantee_2');
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon &.invoice {
    bg-image('invoice_2');
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .icon &.special {
    bg-image('special_2');
  }

  .v-header .detail .detail-wrapper .detail-content .supports .supports-item .text {
    font-size: 12px;
    line-height: 16px;
    font-weight: 200;
  }

  .v-header .detail .detail-wrapper .detail-content .bulletin {
    width: 80%;
    margin: 0 auto;
  }

  .v-header .detail .detail-wrapper .detail-content .bulletin .content {
    margin-bottom: 16px;
    padding: 0 12px;
    line-height: 24px;
    font-size: 12px;
  }

  .v-header .detail .detail-close {
    position: relative;
    width: 32px;
    height: 32px;
    margin: -64px auto 0 auto;
    font-size: 32px;
  }
</style>
