<template>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="menu-item" :class="{current: currentIndex === index}" @click="selectMenu(index, $event)">
            <span class="text border-1px">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="(item, index) in goods" :key="index" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="(food, index) in item.foods" :key="index" class="food-item border-1px" @click="selectFood(food, $event)">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon" alt="">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p v-show="food.description" class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}</span>
                  </div>
                  <div class="price">
                    <span class="now"><span class="uom">￥</span>{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice"><span class="uom">￥</span>{{food.oldPrice}}</span>
                  </div>
                  <div class="cartControl-wrapper">
                    <cartControl @cartAdd="cartAdd($event)" :food="food"></cartControl>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopCart ref="shopCart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopCart>
      <food ref="food" :food="selectedFood"></food>
    </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopCart from '../shopcart/shopcart';
  import cartControl from '../cartcontrol/cartcontrol';
  import food from '../food/food';
  export default {
    props: ['seller'],
    data() {
      return {
        goods: [],
        heightList: [],
        scrollY: 0,
        target: null,
        selectedFood: {}
      };
    },
    components: {
      shopCart,
      cartControl,
      food
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$axios.get('/api/goods').then((res) => {
        this.goods = res.data.data;
        // console.log('this.goods === ' + JSON.stringify(this.goods));
        this.$nextTick(() => {
          this._initScroll();
          this._getHeight();
        });
      });
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.heightList.length; i++) {
          let height1 = this.heightList[i];
          let height2 = this.heightList[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let _selectFoods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              _selectFoods.push(food);
            }
          });
        });
        return _selectFoods;
      }
    },
    methods: {
      selectMenu(idx, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$refs.foodsWrapper.querySelectorAll('.food-list-hook');
        this.foodsScroll.scrollToElement(foodList[idx], 500);
      },
      selectFood(food, event) {
        if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      cartAdd(target) {
        this.$nextTick(() => {
          this.$refs.shopCart.drop(target);
        });
      },
      /*_drop(target) {
        // console.log('goods ---- target === ' + target);
        this.$refs.shopcart.drop(target);
      },*/
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.floor(pos.y));
        });
      },
      _getHeight() {
        let foodList = this.$refs.foodsWrapper.querySelectorAll('.food-list-hook');
        let height = 0;
        this.heightList.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.heightList.push(height);
        }
      }
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
        &.current
          position: relative
          margin-bottom: -1px
          background: #fff
          .text
            font-weight: 700
            border-none()
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
            .desc
              margin-bottom: 8px
            .extra
              .count
                margin-right: 8px
            .price
              line-height: 24px
              .now
                font-size: 14px
                font-weight: 700
                color: rgb(240, 20, 20)
                .uom
                  font-size: 10px
              .old
                font-size: 10px
                font-weight: 700
                text-decoration: line-through
                color: rgb(147, 153, 159)
                .uom
                  font-weight: normal
                  font-size: 10px

            .cartControl-wrapper
              position: absolute
              right: 0
              bottom: 12px
</style>
