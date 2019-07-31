<template>
  <div class="component">
    <div class='list' ref='girdList' :class='[type]'>
      <div class="item-list" v-for='(v,i) in currList' :key='i' v-if='currList.length'>
        <div class='item' v-for='(c, j) in v' :key='j' @click='jump(c.url)'>
          <div class='cover' :style='{backgroundImage: "url(" + c.cover + ")", "height": itemHeight}'><div class="mask"></div></div>
          <div class="txt">
            <div class="item-title" v-if='["gird-2", "gird-1"].indexOf(type) > -1'>{{c.title}}</div>
            <div class="item-tip" v-if='["gird-2", "gird-1"].indexOf(type) > -1'>{{c.tip}}</div>
            <div class='item-tag' v-if='["gird-3", "gird-1"].indexOf(type) > -1'>{{c.tag}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {VueExtend} from 'godspen-lib'
  const rows = {
    'gird-3': 3,
    'gird-2': 2,
    'gird-1': 2
  }
  export default {
    mixins: [VueExtend.mixin],
    name: 'list-grid',
    label: process.env.LABEL,
    style: process.env.STYLE,
    props: {
      type: {
        type: String,
        default: 'gird-1',
        editor: {
          ignore: true
        }
      },
      list: {
        type: Array,
        default () {
          return [
            {
              title: '码良',
              tip: '运满满前端',
              cover: 'https://gw.alipayobjects.com/zos/rmsportal/qnMZzTAViDGQHHjgyICm.png',
              url: '',
              tag: '标签'
            },
            {
              title: '码良',
              tip: '运满满前端',
              cover: 'https://gw.alipayobjects.com/zos/rmsportal/qnMZzTAViDGQHHjgyICm.png',
              url: '',
              tag: '标签'
            }
          ]
        },
        editor: {
          ignore: true
        }
      }
    },
    data () {
      return {
        itemHeight: null
      }
    },
    watch: {
      type (val) {
        this.$nextTick(() => {
          this.itemHeight = `${document.getElementsByClassName('cover')[0].clientWidth * 0.7}px`
        })
      }
    },
    computed: {
      currList () {
        let index = 0
        let list = []
        while (index < this.list.length) {
          list.push(this.list.slice(index, index += rows[this.type]))
        }
        return list
      },
      // itemHeight () {
      //   return this.$refs.girdList
      // }
    },
    mounted () {
      try {
        this.$parent.$el.style.height = 'auto'
      } catch (e) {
        console.log(e)
      }
      this.$nextTick(() => {
        this.itemHeight = `${document.getElementsByClassName('cover')[0].clientWidth * 0.7}px`
      })
    },
    methods: {
      jump (url) {
        if (!url) return
        window.location.href = url
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component {
    width: 100%
    .list{
      width: 100%
      .item-list{
        display flex
        width 100%
        justify-content: space-between
        align-items: flex-start
        .item{
          flex 1
          margin 4px
          position relative
          .cover{
            width 100%
            background-size cover
            background-position center 50%
            background-repeat no-repeat
            position relative
          }
          .mask{
            position: absolute
            top: 0
            left: 0
            right 0
            bottom 0
            background: rgba(0,0,0,.3)
          }
          .txt {
            overflow: hidden
            text-overflow: ellipsis
            white-space:nowrap
            width 100%
            color: #fff
          }
        }
      }
    }
    .gird-3{
      .txt{
        position: absolute
        bottom: 0
        padding: 5px
        font-size: 12px
      }
    }
    .gird-2{
      .txt{
        height 100%
        top 0
        left 0
        bottom 0
        right 0
        position absolute
        display flex
        justify-content: center
        align-items: center
        flex-direction: column
        line-height 1.5
        .item-tip{
          font-size 12px
          margin-top 3px
        }
      }
    }
    .gird-1{
      .item-tag {
        position absolute
        top 5px
        left 5px
        padding 3px 10px
        font-size 12px
        &:after {
          content: '';
          position: absolute;
          top: 0;
          left: 0;
          box-sizing: border-box;
          border-style: solid;
          border-color: #fff;
          -webkit-transform-origin: 50% 50%;
          transform-origin: 50% 50%;
          width: 100%;
          height: 100%
          pointer-events: none;
          border-width: 1px
          border-radius 20px
        }
      }
      .item-title {
        color #333
        margin-top 5px
      }
      .item-tip{
        color #666
        margin 5px 0
        font-size 12px
      }
    }
  }
</style>