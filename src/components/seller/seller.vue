<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2 class="">起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元</div>
          </li>
          <li class="block">
            <h2 class="">商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元</div>
          </li>
          <li class="block">
            <h2 class="">平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>元</div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{active:favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">商家公告</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="supports-item border-1px" v-for="(item,index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picwrapper">
          <ul class="pic-list" ref="piclist">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90" alt="商家实景">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
    <!--<shopcart :min-price="seller.minPrice" :delivery-price="seller.deliveryPrice"></shopcart>-->
  </div>
</template>

<script>
  import iscroll from 'iscroll'
  import star from '../star/star'
  import split from '../split/split'
  import shopcart from '../shopcart/shopcart'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: (() => {
          if (!window.localStorage.favorite) {
            return false;
          } else {
            let favorite = JSON.parse(localStorage.favorite);
            return favorite || false;
          }
        })()
      }
    },
    computed: {
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏';
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    watch: {
      seller: function() {
        this.$nextTick(() => {
          this._initScroll();
          this._initPicscroll();
        })
      }
    },
    mounted() {
      this._initScroll();
      this._initPicscroll();
    },
    methods: {
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new iscroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh();
        }
      },
      _initPicscroll() {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length - margin;
          this.$refs.piclist.style.width = width + 'px';
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new iscroll(this.$refs.picwrapper, {
                scrollX: true,
                eventPassthrough: 'vertical'
              })
            } else {
              this.picScroll.refresh();
            }
          })
        }
      },
      toggleFavorite() {
        this.favorite = !this.favorite;
        let storage = window.localStorage;
        storage.favorite = this.favorite;
      }
    },
    components: {
      star,
      split,
      shopcart
    }
  };
</script>

<style lang="stylus">
  @import "../../common/stylus/mixin.styl"

  .seller {
    position: absolute;
    touch-action: none;
    top: 174px;
    bottom: 48px;
    left: 0;
    width: 100%;
    overflow: hidden;
  }

  .seller .overview {
    position: relative;
    padding: 18px;
  }

  .seller .overview .title {
    margin-bottom: 8px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
  }

  .seller .overview .desc {
    padding-bottom: 18px;
    border-1px(rgba(7, 17, 27, 0.1));
    font-size: 0;
  }

  .seller .overview .desc .star {
    display: inline-block;
    margin-right: 8px;
    vertical-align: top;
  }

  .seller .overview .desc .text {
    display: inline-block;
    margin-right: 12px;
    line-height: 18px;
    vertical-align: top;
    font-size: 10px;
    color: rgb(77, 85, 93);
  }

  .seller .overview .remark {
    display: flex;
    padding-top: 18px;
  }

  .seller .overview .remark .block {
    flex: 1;
    text-align: center;
    border-right: 1px solid rgba(7, 17, 27, 0.1);
  }

  .seller .overview .remark .block &:last-child {
    border: none;
  }

  .seller .overview .remark .block h2 {
    margin-bottom: 4px;
    line-height: 10px;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }

  .seller .overview .remark .block .content {
    line-height: 24px;
    font-size: 10px;
    color: rgb(7, 17, 27);
  }

  .seller .overview .remark .block .content .stress {
    font-size: 24px;
  }

  .seller .overview .favorite {
    position: absolute;
    width: 50px;
    right: 11px;
    top: 18px;
    text-align: center;
  }

  .seller .overview .favorite .icon-favorite {
    display: block;
    margin-bottom: 4px;
    line-height: 24px;
    font-size: 24px;
    color: #d4d6d9;
  }
  
  .seller .overview .favorite .icon-favorite:active {
    color: rgb(240, 20, 20);
  }

  .seller .overview .favorite .text {
    line-height: 10px;
    font-size: 10px;
    color: rgb(77, 85, 93);
  }

  .seller .bulletin {
    padding: 18px 18px 0 18px;
  }

  .seller .bulletin .title {
    margin-bottom: 8px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
  }

  .seller .bulletin .content-wrapper {
    padding: 0 12px 16px 12px;
    border-1px(rgba(7, 17, 27, 0.1));
  }

  .seller .bulletin .content-wrapper .content {
    line-height: 24px;
    font-size: 12px;
    color: rgb(240, 20, 20);
  }

  .seller .bulletin .supports .supports-item {
    padding: 16px 12px;
    border-1px(rgba(7, 17, 27, 0.1));
    font-size: 0;
  }

  .seller .bulletin .supports .supports-item &:last-child {
    border-none();
  }

  .seller .bulletin .supports .supports-item .icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 6px;
    vertical-align: top;
    background-size: 16px 16px;
    background-repeat: no-repeat;
  }

  .seller .bulletin .supports .supports-item .icon &.discount {
    background:url("discount_4@3x.png");
  }

  .seller .bulletin .supports .supports-item .icon &.decrease {
    background:url("decrease_4@3x.png");
  }

  .seller .bulletin .supports .supports-item .icon &.guarantee {
    background:url("guarantee_4@3x.png");
  }

  .seller .bulletin .supports .supports-item .icon &.invoice {
    background:url("invoice_4@3x.png");
  }

  .seller .bulletin .supports .supports-item .icon &.special {
    background:url("special_4@3x.png");
  }

  .seller .bulletin .supports .supports-item .text {
    line-height: 16px;
    font-size: 12px;
    color: rgb(7, 17, 27);
  }

  .seller .pics {
    padding: 18px;
  }

  .seller .pics .title {
    margin-bottom: 8px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
  }

  .seller .pics .pic-wrapper {
    width: 100%;
    overflow: hidden;
    white-space: nowrap;
  }

  .seller .pics .pic-wrapper .pic-list {
    font-size: 0;
  }

  .seller .pics .pic-wrapper .pic-list .pic-item {
    display: inline-block;
    margin-right: 6px;
    width: 120px;
    height: 90px;
  }

  .seller .pics .pic-wrapper .pic-list .pic-item &:last-child {
    margin: 0;
  }

  .seller .info {
    margin: 18px 18px 0 18px;
  }

  .seller .info .title {
    padding-bottom: 12px;
    line-height: 14px;
    border-1px(rgba(7, 17, 27, 0.1));
    color: rgb(7, 17, 27);
    font-size: 14px;
  }

  .seller .info .info-item {
    padding: 16px 12px;
    line-height: 16px;
    border-1px(rgba(7, 17, 27, 0.1));
    font-size: 12px;
  }

  .seller .info .info-item &:last-child {
    border-none();
  }
</style>
