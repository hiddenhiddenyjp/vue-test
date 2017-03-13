<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img width="64" height="64" :src="seller.avatar">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count"  @click="showDetail">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        
        <div class="bulletin-wrapper">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>

        <div class="background">
           <img :src="seller.avatar" alt="" width="100%" height= "100%">
        </div>

        <div v-show="detailShow" class="detail" transition="fade">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size="48" :score="seller.score"></star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class="supports">
                        <li class="support-item" v-for="item in seller.supports">
                          <span class="icon" :class="classMap[seller.supports[$index].type]">
                          </span>
                          <span class="text">
                            {{seller.supports[$index].description}}
                          </span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="bulletin">
                         <div class="text">{{seller.bulletin}}</div>
                    </div>
                </div>
            </div>
            <div class="detail-close" @click="hideDetail">
                <div class="icon-close">
                </div>
            </div>
        </div>
    </div>
</template>

<script type= "text/ecmascript-6">
    import star from 'components/star/star';
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                detailShow: false
            };
        },
        created() {
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        },
        methods: {
            showDetail() {
               this.detailShow = true;
            },
            hideDetail() {
              this.detailShow = false;
            }
        },
        components: {
          star
        }
    }
</script>

<style lang= "stylus" rel="stylesheet/stylus">
    .header
      color: #fff
      position: relative
      background: rgba(7,17,27,0.5)
      overflow: hidden
      .content-wrapper
        padding: 24px 12px 18px 24px
        font-size: 0
        position: relative
        .avatar
          display: inline-block
          border-radius: 2px
        .content
          display: inline-block
          font-size: 14px
          margin-left: 16px
          .title
            margin: 2px 0 8px 0
            .brand
              width: 30px
              height: 18px
              display: inline-block
              background-image: url(./brand@2x.png)
              background-size: 30px 18px
              background-repeat: no-repeat
            .name
              font-size: 16px
              color: rgb(255,255,255)
              font-weight: bold
              line-height: 18px
              margin-left: 6px
              vertical-align: top
          .description
            font-size: 12px
            color: rgb(255,255,255)
            font-weight: 100
            line-height: 12px
            margin-bottom: 10px
          .support
            margin-bottom: 2px
            .icon
                display: inline-block
                width: 12px
                height: 12px
                margin-right: 4px
                background-size: 12px 12px
                background-repeat: no-repeat
                &.decrease
                  background-image: url(./decrease_2@2x.png)
                &.discount
                  background-image: url(./discount_2@2x.png)
                &.guarantee
                  background-image: url(./guarantee_2@2x.png)
                &.invoice
                  background-image: url(./invoice_2@2x.png)
                &.special
                  background-image: url(./special_2@2x.png)
            .text
              font-size: 12px
              font-weight: 200
              line-height: 12px
              vertical-align: top
              display: inline-block
        .support-count
          position: absolute
          right: 12px
          bottom: 18px
          height: 24px
          background-color:rgba(0,0,0,0.2)
          border-radius: 8px
          padding:0 8px
          .count
            font-size: 10px
            font-weight: 200
            line-height: 24px
            vertical-algin: top
            color: #fff
          .icon-keyboard_arrow_right
            font-size: 10px
            line-height: 24px
            margin-left: 2px
            text-align: center
      .bulletin-wrapper
        height:28px
        line-height: 28px
        padding: 0 22px 0 12px
        white-space: nowrap
        overflow: hidden
        text-overflow: ellipsis
        background-color: rgba(7,17,27,0.2)
        position: relative
        .bulletin-title
          width: 22px
          height: 12px
          display: inline-block
          background-image: url(bulletin@2x.png)
          background-size: 22px 12px
        .bulletin-text
          font-size: 10px
          margin-left: 4px
        .icon-keyboard_arrow_right
          position: absolute
          right: 8px
          bottom: 6px
      .background
        position: absolute
        z-index: -1
        top: 0
        left: 0
        width: 100%
        height: 100%
        filter: blur(10px)
      .detail
        position: fixed
        z-index: 100
        width: 100%
        height: 100%
        overflow: auto
        top: 0
        left: 0
        transition: all 0.5s
        &.fade-transition
          opacity: 1
          background: rgba(7,17,27,0.8)
        &.fade-enter, &.fade-leave
          opacity: 0
          background: rgba(7,17,27,0)
        .detail-wrapper
          min-height: 100%
          width: 100%
          .detail-main
            margin-top: 64px
            padding-bottom: 64px
            .name
              line-height: 16px
              font-weight: 700
              font-size: 16px
              color: #fff
              text-align: center
            .star-wrapper
              margin: 16px 0 28px 0
              text-align: center
            .title
              display: flex
              width: 80%
              margin: 28px auto 24px
              .line
                flex: 1
                border-bottom : 1px solid rgba(255,255,255,0.2)
                position: relative
                top: -6px
              .text
                margin: 0 12px
            .supports
               width: 80%
               margin: 24px auto 28px
               .support-item
                 padding: 0 12px
                 font-size: 0
                 margin-bottom: 12px
                 .icon
                   display: inline-block
                   width: 16px
                   height: 16px
                   background-size: 16px 16px
                   background-repeat: no-repeat
                   &.decrease
                     background-image: url(./decrease_2@2x.png)
                   &.discount
                     background-image: url(./discount_2@2x.png)
                   &.guarantee
                     background-image: url(./guarantee_2@2x.png)
                   &.invoice
                     background-image: url(./invoice_2@2x.png)
                   &.special
                     background-image: url(./special_2@2x.png)
                 .text
                   margin-left:6px
                   vertical-align: top
                   font-size: 12px
                   font-weight: 200
                   line-height: 16px
            .bulletin
               padding: 0 12px
               width: 80%
               margin: 0 auto
               .text
                  font-size: 12px
                  font-weight: 200
                  color:rgb(255,255,255)
                  line-height: 24px

        .detail-close
            position: relative
            width: 32px
            height: 32px
            margin: -64px auto 0 auto
            clear: both
            font-size: 32px

</style>