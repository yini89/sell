<template>
  <transition name="move">
    <div class="food" v-show="isShow" ref="wrapper">
      <div class="food-wrapper">
        <div class="img-header">
          <img :src="food.image" alt="">
          <span class="icon-arrow_lift" @click="back"></span>
        </div>
        <div class="food-content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="new"><span class="uom">￥</span>{{food.price}}</span>
            <span class="old" v-show="food.oldPrice"><span class="uom">￥</span>{{food.oldPrice}}</span>
          </div>
          <transition name="fade">
            <div class="buy" @click="addFirst($event)" v-show="!food.count || food.count === 0">加入购物车</div>
          </transition>
          <div class="cartControl-wrapper">
            <cartControl :food="food"></cartControl>
          </div>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split v-show="food.info"></split>
        <div class="ratings">
          <h1 class="title">商品评价</h1>
          <ratingSelect @changeSelectType="changeSelectType" @toggleOnlyContent="toggleOnlyContent" :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"></ratingSelect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(item.rateType, item.text)" class="rating-item border-1px" v-for="(item, index) in food.ratings" :key="index">
                <div class="user">
                  <span class="name">{{item.username}}</span>
                  <img class="avatar" :src="item.avatar" alt="" width="12" height="12">
                </div>
                <p class="time">{{item.rateTime | formatDate}}</p>
                <p class="text">
                  <span :class="{'icon-thumb_up': item.rateType === 0, 'icon-thumb_down': item.rateType === 1}"></span>{{item.text}}
                </p>
              </li>
            </ul>
            <div v-show="!food.ratings || !food.ratings.length" class="no-rating">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import cartControl from '../cartcontrol/cartcontrol';
  import BScroll from 'better-scroll';
  import {formatDate} from "../../common/js/date";
  import split from '../split/split';
  import ratingSelect from '../ratingselect/ratingselect';
  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        isShow: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    components: {
      cartControl,
      split,
      ratingSelect
    },
    methods: {
      show() {
        this.isShow = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.wrapper, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        this.$set(this.food, 'count', 1);
        this.$emit('cartAdd', event.target);
      },
      changeSelectType(type) {
        this.$nextTick(() => {
          this.selectType = type;
        });
      },
      toggleOnlyContent(only) {
        this.$nextTick(() => {
          this.onlyContent = !only;
        });
      },
      back() {
        this.isShow = false;
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    }
  };
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .food
    position: fixed
    left: 0
    top: 0
    bottom: 48px
    width: 100%
    background: #fff
    z-index: 30
    &.move-enter-active, &.move-leave-active
      transition: all 0.3s linear
      transform: translate3d(0, 0, 0)
    &.move-enter, &.move-leave-to
      transform: translate3d(100%, 0, 0)
    .img-header
      position: relative
      width: 100%
      height: 0
      padding-bottom: 100%
      img
        position: absolute
        left: 0
        top: 0
        width: 100%
        height: 100
      .icon-arrow_lift
        position: absolute
        left: 0
        top: 0
        padding: 10px
        color: #fff
    .food-content
      position: relative
      padding: 18px
      background-color: #fff
      .title
        line-height: 14px
        font-size: 14px
        font-weight: 700
        color: rgb(7, 17, 27)
      .detail
        margin: 8px 0 18px 0
        line-height: 10px
        font-size: 0
        color: rgb(147, 153, 159)
        .sell-count
          margin-right: 12px
          font-size: 10px
        .rating
          font-size: 10px
      .price
        line-height: 24px
        font-size: 0
        .new
          margin-right: 12px
          font-size: 14px
          font-weight: 700
          color: rgb(240, 20, 20)
          .uom
            font-size: 10px
            font-weight: normal
        .old
          font-size: 10px
          font-weight: 700
          text-decoration: line-through
          color: rgb(147, 153, 159)
          .uom
            font-weight: normal
      .buy
        position: absolute
        right: 18px
        bottom: 18px
        padding: 6px 12px
        line-height: 24px
        font-size: 10px
        color: #fff
        -webkit-border-radius: 24px
        -moz-border-radius: 24px
        border-radius: 24px
        background: rgb(0, 160, 220)
        z-index: 10
        &.fade-enter-active, &.fade-leave-active
          transition: all 0.3s
          opacity: 1
        &.fade-enter, &.fade-leave-to
          opacity: 0
      .cartControl-wrapper
        position: absolute
        right: 18px
        bottom: 18px
    .info
      padding: 18px
      background-color: #fff
      .title
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
        font-weight: 500
      .text
        margin-top: 6px
        padding: 0 8px
        line-height: 24px
        font-size: 12px
        color: rgb(77, 85, 93)
    .ratings
      padding: 18px 0
      background: #fff
      .title
        padding-left: 18px
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
        font-weight: 500
      .rating-wrapper
        padding: 0 18px
        .rating-item
          position: relative
          padding: 16px 0
          border-1px(rgba(7, 17, 27, 0.1))
          .user
            position: absolute
            right: 18px
            top: 12px
            font-size: 0
            .name
              display: inline-block
              vertical-align: top
              margin-right: 6px
              line-height: 12px
              font-size: 10px
              color: rgb(147, 153, 159)
            .avatar
              border-radius: 50%
          .time
            line-height: 12px
            font-size: 10px
            color: rgb(147, 153, 159)
          .text
            margin-top: 6px
            line-height: 16px
            font-size: 12px
            color: rgb(7, 17, 27)
            .icon-thumb_up, .icon-thumb_down
              display: inline-block
              margin-right: 4px
              vertical-align: top
              font-size: 12px
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .icon-thumb_down
              color: rgb(147, 153, 159)
        .no-rating
          padding: 16px 0
          font-size: 12px
          color: rgb(147, 153, 159)
          text-align: center
</style>
