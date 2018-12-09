<template>
    <div class="rating" ref="ratings">
      <div class="rating-content">
        <div class="over-view">
          <div class="over-view-left">
            <h1 class="score">{{seller.score}}</h1>
            <div class="title">综合评分</div>
            <div class="rank">高于周边商家{{seller.rankRate}}%</div>
          </div>
          <div class="over-view-right">
            <div class="score-wrapper">
              <span class="title">服务态度</span>
              <star :size="36" :score="seller.serviceScore"></star>
              <span class="score">{{seller.serviceScore}}</span>
            </div>
            <div class="score-wrapper">
              <span class="title">商品评分</span>
              <star :size="36" :score="seller.foodScore"></star>
              <span class="score">{{seller.foodScore}}</span>
            </div>
            <div class="delivery-wrapper">
              <span class="title">送达时间</span>
              <span class="delivery">{{seller.deliveryTime}}分钟</span>
            </div>
          </div>
        </div>
        <split></split>
        <ratingSelect @changeSelectType="changeSelectType" @toggleOnlyContent="toggleOnlyContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="ratings"></ratingSelect>
        <div class="rating-wrapper">
          <ul>
            <li v-show="needShow(item.rateType, item.text)" class="rating-item border-1px" v-for="(item, index) in ratings" :key="index">
              <div class="avatar">
                <img :src="item.avatar" alt="" width="28" height="28">
              </div>
              <div class="content">
                <h1 class="name">{{item.username}}</h1>
                <div class="star-wrapper">
                  <star :size="24" :score="item.score"></star>
                  <span v-show="item.deliveryTime" class="delivery">{{item.deliveryTime}}分钟送达</span>
                </div>
                <p class="text">{{item.text}}</p>
                <div class="recommend">
                  <span :class="{'icon-thumb_up': item.rateType === 0, 'icon-thumb_down': item.rateType === 1}"></span>
                  <span class="item" v-for="(rec, idx) in item.recommend" :key="idx">{{rec}}</span>
                </div>
                <div class="time">{{item.rateTime | formatDate}}</div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
  import star from '../star/star';
  import split from '../split/split';
  import ratingSelect from '../ratingselect/ratingselect';
  import BScroll from 'better-scroll';
  import {formatDate} from "../../common/js/date";
  /*const POSITIVE = 0;
  const NEGATIVE = 1;*/
  const ALL = 2;
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      };
    },
    created() {
      this.$axios.get('/api/ratings').then((res) => {
        this.ratings = res.data.data;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      });
    },
    components: {
      star,
      split,
      ratingSelect
    },
    methods: {
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
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
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
  .rating
    position: absolute
    left: 0
    top: 178px
    bottom: 0
    width: 100%
    background: #fff
    overflow: hidden
    .over-view
      display: flex
      padding: 18px 0
      background-color: #fff
      .over-view-left
        flex: 0 0 137px
        padding: 6px 0
        width: 137px
        border-right: 1px solid rgba(7, 17, 27, 0.1)
        text-align: center
        @media only screen and (max-width: 320px)
          flex: 0 0 120px
          width: 120px
        .score
          line-height: 28px
          font-size: 24px
          color: rgb(255, 153, 0)
        .title
          margin: 6px 0 8px 0
          line-height: 12px
          font-size: 12px
          color: rgb(7, 17, 27)
        .rank
          line-height: 10px
          font-size: 10px
          color: rgb(147, 153, 159)
      .over-view-right
        flex: 1
        padding: 6px 0 6px 24px
        @media only screen and (max-width: 320px)
          padding-left: 6px
        .score-wrapper
          margin-bottom: 8px
          .title
            display: inline-block
            vertical-align: top
            line-height: 18px
            font-size: 12px
            color: rgb(7, 17, 27)
          .star
            display: inline-block
            vertical-align: top
            margin: 0 12px
          .score
            display: inline-block
            vertical-align: top
            line-height: 18px
            font-size: 12px
            color: rgb(255, 153, 0)
        .delivery-wrapper
          font-size: 0
          .title
            margin-right: 12px
            line-height: 18px
            font-size: 12px
            color: rgb(7, 17, 27)
          .delivery
            line-height: 18px
            font-size: 12px
            color: rgb(147, 153, 159)
    .rating-wrapper
      padding: 0 18px
      .rating-item
        display: flex
        padding: 18px 0
        border-1px(rgba(7, 17, 27, 0.1))
        .avatar
          flex: 0 0 28px
          width: 28px
          margin-right: 12px
          img
            border-radius: 50%
        .content
          position: relative
          flex: 1
          .name
            line-height: 12px
            font-size: 10px
            color: rgb(7, 17, 27)
            font-weight: 200
          .star-wrapper
            margin: 4px 0 6px 0
            font-size: 0
            .star
              display: inline-block
              vertical-align: top
              margin-right: 6px
            .delivery
              line-height: 12px
              font-size: 10px
              color: rgb(147, 153, 159)
          .text
            line-height: 18px
            font-size: 12px
            color: rgb(7, 17, 27)
          .recommend
            margin-top: 8px
            line-height: 16px
            font-size: 0
            .icon-thumb_up, .icon-thumb_down
              display: inline-block
              vertical-align: top
              margin-right: 8px
              font-size: 12px
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .icon-thumb_down
              color: rgb(147, 153, 159)
            .item
              display: inline-block
              margin-right: 8px
              font-size: 9px
              padding: 0 6px
              color: rgb(147, 153, 159)
              background: #fff
              border: 1px solid rgba(7, 17, 27, 0.1)
              border-radius: 2px

          .time
            position: absolute
            right: 0
            top: 0
            line-height: 12px
            font-size: 12px
            color: rgb(147, 153, 159)
</style>
