<template>
    <div class="shopcart">
      <div class="content">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo">
              <i class="icon-shopping_cart"></i>
            </div>
          </div>
          <div class="price">￥0</div>
          <div class="desc">配送费</div>
        </div>
        <div class="content-right"></div>
      </div>
    </div>
</template>

<script>
  export default {};
</script>

<style scoped lang="stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width: 100%
    height: 48px
    .content
      display: flex
      width: 100%
      height: 100%
      .content-left
        flex: 1
        background: #000
        .logo-wrapper
          position: relative
          top: -10px
          display: inline-block
          width: 56px
          height: 56px
          margin: 0 12px
          padding: 6px
          border-radius: 50%
          box-sizing: border-box
          background: #000
          text-align: center
          .logo
            width: 44px
            height: 44px
            border-radius: 50%
            background: rgba(255, 255, 255, 0.2)
            .icon-shopping_cart
              line-height: 44px
              font-size: 24px
              color: rgba(255, 255, 255, 0.4)
        .price
          display: inline-block
          vertical-align: top
          margin-top: 12px
          padding: 0 12px 0 0
          font-size: 16px
          color: rgba(255, 255, 255, 0.4)
          font-weight: 700
          line-height: 24px
          border-right: 1px solid rgba(255, 255, 255, 0.1)
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          font-size: 12px
          color: rgba(255, 255, 255, 0.4)
      .content-right
        flex: 0 0 105px
        width: 105px
        background: #666
</style>
