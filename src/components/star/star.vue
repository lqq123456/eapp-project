<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    data() {
      return {};
    },
    computed: {
      starType() {
        return 'star_' + this.size
      },
      itemClasses() {
        let itemClasses = [];
        let score = Math.floor(this.score * 2) / 2;
        for (let i = 1; i <= score; i++) {
          itemClasses.push('on');
          // for(let i = 0; i < 5-this.score; i++){
          //   itemClasses.push('off')
          // }
        }
        if (score % 1 != 0) {
          itemClasses.push('half');
        }
        while (itemClasses.length < 5) {
          itemClasses.push('off');
        }
        return itemClasses;
      }
    }
  }
</script>

<style lang="stylus">
  @import "../../common/stylus/mixin.styl"

  .star {
    font-size: 0;
  }

  .star .star-item {
    display: inline-block;
    background-repeat: no-repeat;
  }

  &.star_48 .star-item {
    margin-right: 22px;
    width: 20px;
    height: 20px;
    background-size: 20px 20px;
  }

  &.on {
    background: url(star48_on@3x.png);
  }

  &.off {
    background: url(star48_off@3x.png);
  }

  &.half {
    background: url('star48_half@3x.png');
  }

  &:last-child {
    margin: 0;
  }

  &.star_36 .star-item {
    margin-right: 6px;
    width: 15px;
    height: 15px;
    background-size: 15px 15px;
  }

  &.on {
    background: url('star36_on@3x.png');
  }

  &.off {
    background: url('star36_off@3x.png');
  }

  &.half {
    background: url('star36_half@3x.png');
  }

  &:last-child {
    margin: 0;
  }

  &.star_24 .star-item {
    margin-right: 3px;
    width: 10px;
    height: 10px;
    background-size: 10px 10px;
  }

  &.on {
    background: url('star24_on@3x.png');
  }

  &.off {
    background: url('star24_off@3x.png');
  }

  &.half {
    background: url('star24_half@3x.png');
  }

  &:last-child {
    margin: 0;
  }
</style>
