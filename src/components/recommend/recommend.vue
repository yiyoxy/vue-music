<template>
  <div class="recommend">
    <div class="recommend-content">
      <div v-if="recommends.length" class="slider-wrapper">
        <slider>
          <div v-for="(item,index) in recommends" :key="index">
            <a :href="item.linkUrl">
              <img :src="item.picUrl" alt="">
            </a>
          </div>
        </slider>
      </div>
      <div class="recommend-list">
        <h1 class="list-title">热门歌单推荐</h1>
  
        <ul>
           <li class="item" v-for="(item, index) in discList" :key="index">
            <div class="icon">
              <img width="60" height="60" :src="item.imgurl" alt="">
            </div>
            <div class="text">
              <h1 class="name">{{item.creator.name}}</h1>
              <p class="desc">
                {{item.dissname}}
              </p>
            </div>
          </li> 
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import { getRecommend, getDiscList } from '@/api/recommend'
import { ERR_OK } from '@/api/config'

export default {
  data() {
    return {
      recommends: [],
      discList: []
    }
  },
  created() {
    this._getRecommend()
    this._getDiscList()
  },
  methods: {
    _getRecommend() {
      getRecommend().then((res) => {
        if (res.code === ERR_OK) {
          // console.log(res.data.slider)
          this.recommends = res.data.slider
        }
      })
    },
    _getDiscList() {
      getDiscList().then((res) => {
        if (res.code === ERR_OK) {
          this.discList = res.data.list
        }
      })
    }
  },
  components: {
    'slider': require('@/base/slider/slider.vue')
  }

}
</script>
<style lang="stylus" scoped>
  @import "~common/stylus/variable"
  .recommend
    position fixed
    width 100%
    top 88px
    bottom 0
    .recommend-content
      height 100%
      // overflow hidden
      .slider-wrapper
        position relative
        width 100%
        // overflow hidden
      .recommend-list
        .list-title
          height 65px
          line-height 65px
          text-align center
          font-size $font-size-medium
          color $color-theme
        .item
          display flex
          box-sizing border-box
          align-items center
          padding 0 20px 20px 20px
          .icon
            flex 0 0 60px
            width 60px
            padding-right 20px
          .text
            display flex
            flex-direction column
            justify-content center
            flex 1
            line-height 20px
            overflow hidden
            font-size $font-size-medium
            .name
              margin-bottom 10px
              color $color-text
            .desc
              color $color-text-d
</style>

