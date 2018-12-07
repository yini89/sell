<template>
    <div class="rating">
      <div class="rating-wrapper">
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
      </div>
    </div>
</template>

<script>
  import star from '../star/star';
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      star
    }
  };  
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .rating
    position: absolute
    left: 0
    top: 178px
    bottom: 0
    width: 100%
    background: #fff
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
</style>
