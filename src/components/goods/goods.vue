<template>
    <div class="goods">
      <div class="menu-wrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="menu-item">
            <span class="text border-1px">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="food-list">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="(food, index) in item.foods" :key="index" class="food-item border-1px">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon" alt="">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p v-show="food.description" class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span>月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}</span>
                  </div>
                  <div class="price">
                    <span>￥{{food.price}}</span>
                    <span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  export default {
    props: ['seller'],
    data() {
      return {
        goods: []
      };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$axios.get('/api/goods').then((res) => {
        this.goods = res.data.data;
      });
    }
  };
</script>

<style scoped lang="stylus">
  @import "../../common/stylus/mixin.styl";
  .goods
    display: flex
    position: absolute
    width: 100%
    top: 178px
    bottom: 52px
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        height: 54px
        width: 56px
        line-height: 14px
        padding: 0 12px
        .text
          display: table-cell
          font-size: 12px
          border-1px(rgba(7, 17, 27, 0.1))
          vertical-align: middle
          .icon
            display: inline-block
            width: 12px
            height: 12px
            margin-right: 2px
            background-size: 12px 12px
            background-repeat: no-repeat
            vertical-align: top
            &.decrease
              bg-img('decrease_3')
            &.discount
              bg-img('discount_3')
            &.guarantee
              bg-img('guarantee_3')
            &.invoice
              bg-img('invoice_3')
            &.special
              bg-img('special_3')
    .foods-wrapper
      flex: 1
      .food-list
        .title
          height: 26px
          line-height: 26px
          font-size: 12px
          color: rgb(147, 153, 159)
          background: #f3f5f7
          padding: 0 14px
          border-left: 2px solid #d9dde1
        .food-item
          display: flex
          margin: 18px
          border-1px(rgba(7, 17, 27, 0.1))
          padding-bottom: 18px
          &:last-child
            border-none()
            margin-bottom: 0
          .icon
            flex: 0 0 57px
          .content
            flex: 1
            margin-left: 10px
            .name
              height: 14px
              line-height: 14px
              font-size: 14px
              color: rgb(7, 17, 27)
              margin: 2px 0 8px 0
            .desc, .extra
              height: 10px
              line-height: 10px
              font-size: 10px
              color: rgb(147, 153, 159)
            .extra
              span
                &:last-child
                  color: red
</style>
