<template>
  <div class="rating-select">
    <div class="rating-type border-1px">
      <span :class="{active: selectType === 2}" class="block positive" @click="select(2, $event)">{{desc.all}}<span class="count">57</span></span>
      <span :class="{active: selectType === 0}" class="block positive" @click="select(0, $event)">{{desc.positive}}<span class="count">50</span></span>
      <span :class="{active: selectType === 1}" class="block negative" @click="select(1, $event)">{{desc.negative}}<span class="count">7</span></span>
    </div>
    <p class="switch" :class="{on: onlyContent}" @click="toggle">
      <span class="icon-check_circle"></span>只看有内容的评价
    </p>
  </div>
</template>

<script>
  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      },
      selectType: {
        type: Number,
        default() {
          return ALL;
        }
      },
      onlyContent: {
        type: Boolean,
        default() {
          return false;
        }
      },
      desc: {
        type: Object,
        default() {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          };
        }
      }
    },
    methods: {
      select(type, event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('changeSelectType', type);
      },
      toggle() {
        if (!event._constructed) {
          return;
        }
        this.$emit('toggleOnlyContent', this.onlyContent);
      }
    }
  };
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .rating-select
    .rating-type
      padding: 18px 0
      margin: 0 18px
      font-size: 0
      color: rgb(77, 85, 93)
      border-1px(rgba(7, 17, 27, 0.1))
      .block
        display: inline-block
        padding: 8px 12px
        margin-right: 8px
        line-height: 16px
        font-size: 12px
        border-radius: 2px
        color: rgb(77, 85, 93)
        &.positive
          background-color: rgba(0, 160, 220, 0.2)
          &.active
            color: #fff
            background-color: rgb(0, 160, 220)
        &.negative
          background-color: rgba(77, 85, 93, 0.2)
          &.active
            color: #fff
            background-color: rgb(77, 85, 93)
        .count
          margin-left: 4px
          font-size: 8px

    .switch
      padding:12px 18px
      line-height: 24px
      color: rgb(147, 153, 159)
      border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      font-size: 12px
      &.on
        .icon-check_circle
          color: rgb(0, 160, 220)
      .icon-check_circle
        display: inline-block
        margin-right: 4px
        vertical-align: top
        font-size: 24px

</style>
