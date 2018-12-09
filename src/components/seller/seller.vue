<template>
    <div class="seller" ref="seller">
      <div class="seller-content">
        <div class="over-view border-1px">
          <h1 class="title">{{seller.name}}</h1>
          <div class="desc">
            <star :size="36" :score="seller.score"></star>
            <span class="text">({{seller.ratingCount}})</span>
            <span class="text">月售{{seller.sellCount}}单</span>
          </div>
          <ul class="remark">
            <li class="block">
              <h2>起送价</h2>
              <div class="content">
                <span class="stress">{{seller.minPrice}}</span>元
              </div>
            </li>
            <li class="block">
              <h2>商家配送</h2>
              <div class="content">
                <span class="stress">{{seller.deliveryPrice}}</span>元
              </div>
            </li>
            <li class="block">
              <h2>平均配送时间</h2>
              <div class="content">
                <span class="stress">{{seller.deliveryTime}}</span>分钟
              </div>
            </li>
          </ul>
          <div class="favorite" @click="toggleFavorite">
            <span class="icon-favorite" :class="{active: favorite}"></span>
            <span class="text">{{favoriteText}}</span>
          </div>
        </div>
        <split></split>
        <div class="bulletin">
          <h1 class="title">公告与活动</h1>
          <div class="content-wrapper border-1px">
            <p class="content">{{seller.bulletin}}</p>
          </div>
          <ul v-show="seller.supports" class="supports">
            <li v-for="(item, index) in seller.supports" :key="index" class="support-item border-1px">
              <span class="icon" :class="classMap[item.type]"></span>
              <span class="text">{{item.description}}</span>
            </li>
          </ul>
        </div>
        <split></split>
        <div class="pics">
          <h1 class="title">商家实景</h1>
          <div class="pic-wrapper" ref="picWrapper">
            <ul class="pic-list" ref="picList">
              <li class="pic-item" v-for="(item, index) in seller.pics" :key="index">
                <img :src="item" width="120" height="90">
              </li>
            </ul>
          </div>
        </div>
        <split></split>
        <div class="info">
          <h1 class="title border-1px">商家信息</h1>
          <ul>
            <li class="info-item border-1px" v-for="(item, index) in seller.infos" :key="index">{{item}}</li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import star from '../star/star';
  import split from '../split/split';
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: false
      };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    computed: {
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏';
      }
    },
    watch: {
      'seller'() {
        this._initScroll();
        this._initPics();
      }
    },
    components: {
      star,
      split
    },
    mounted() {
      this._initScroll();
      this._initPics();
    },
    methods: {
      toggleFavorite(event) {
        if (!event._constructed) {
          return;
        }
        this.favorite = !this.favorite;
      },
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      },
      _initPics() {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = this.seller.pics.length * (picWidth + margin) - margin;
          this.$refs.picList.style.width = width + 'px';
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true, //表示横向滚动
                eventPassthrough: 'vertical' //表示横向滚动的时候就忽略纵向滚动
              });
            } else {
              this.picScroll.refresh();
            }
          });
        }
      }
    }
  };
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .seller
    position: absolute
    left: 0
    top: 178px
    bottom: 0
    width: 100%
    overflow: hidden
    .over-view
      position: relative
      padding: 18px
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
      .desc
        line-height: 36px
        font-size: 0
        border-1px(rgba(7, 17, 27, 0.1))
        .star
          display: inline-block
          vertical-align: top
          margin-right: 8px
        .text
          margin-right: 12px
          line-height: 18px
          font-size: 10px
          color: rgb(77, 85, 93)
      .remark
        display: flex
        padding-top: 18px
        .block
          flex: 1
          border-right: 1px solid rgba(7, 17, 27, 0.1)
          text-align: center
          &:last-child
            border-right: 0
          h2
            margin-bottom: 4px
            line-height: 10px
            font-size: 10px
            color: rgb(147, 153, 159)
          .content
            line-height: 24px
            font-size: 10px
            font-weight: 200
            color: rgb(7, 17, 27)
            .stress
              font-size: 24px
      .favorite
        position: absolute
        right: 11px
        top: 18px
        width: 50px
        text-align: center
        .icon-favorite
          display: block
          line-height: 24px
          font-size: 24px
          color: #d4d6d9
          &.active
            color: rgb(240, 20, 20)
        .text
          display: block
          margin-top: 4px
          line-height: 10px
          font-size: 10px
          color: rgb(77, 85, 93)
    .bulletin
      padding: 18px 18px 0 18px
      background: #fff
      .title
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
      .content-wrapper
        margin-top: 8px
        padding: 0 12px 16px 12px
        border-1px(rgba(7, 17, 27, 0.1))
        .content
          line-height: 24px
          font-size: 12px
          color: rgb(240, 20, 20)
      .supports
        .support-item
          padding: 16px 12px
          border-1px(rgba(7, 17, 27, 0.1))
          font-size: 0
          &:last-child
            border-none()
          .icon
            display: inline-block
            vertical-align: top
            width: 16px
            height: 16px
            margin-right: 6px
            background-size: 16px 16px
            background-repeat: no-repeat
            &.decrease
              bg-img('decrease_4')
            &.discount
              bg-img('discount_4')
            &.special
              bg-img('special_4')
            &.invoice
              bg-img('invoice_4')
            &.guarantee
              bg-img('guarantee_4')
          .text
            line-height: 16px
            font-size: 12px
            color: rgb(7, 17, 27)
    .pics
      padding: 18px
      .title
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
      .pic-wrapper
        margin-top: 12px
        width: 100%
        overflow: hidden
        .pic-list
          white-space: nowrap
          font-size: 0
          .pic-item
            display: inline-block
            margin-right: 6px
            width: 120px
            height: 90px
            &:last-child
              margin-right: 0
    .info
      padding: 18px
      .title
        margin-bottom: 12px
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
        border-1px(rgba(7, 17, 27, 0.1))
      .info-item
        padding: 16px 12px
        line-height: 16px
        font-size: 12px
        color: rgb(7, 17, 27)
        border-1px(rgba(7, 17, 27, 0.1))
        &:last-child
          border-none()
</style>
