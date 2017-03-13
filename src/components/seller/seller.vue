<template>
    <div class="seller" v-el:seller>
        <div class="seller-content">
            <div class="overview">
                <h1 class="title">{{seller.name}}</h1>
                <div class="desc">
                    <star :size="36" :score="seller.score"></star>  
                    <span class="text">({{seller.ratingCount}})</span>
                    <span class="text">月售{{seller.sellCount}}单</span> 
                </div>
                <ul class="remark">
                    <li class="block">
                        <h2 class="title">起送价</h2>
                        <div class="content">
                            <span class="stress">{{seller.minPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <h2 class="title">商家配送</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <h2 class="title">平均配送时间</h2>
                        <div class="content">
                            <span class="stress">{{seller.deliveryTime}}</span>分钟
                        </div>
                    </li>
                </ul>
                <div class="favorite" @click="toggleFavorite">
                    <span class="icon-favorite" :class="{'active':favorite}"></span>
                    <span class="text">{{favoriteText}}</span>
                </div>
            </div>
            <split></split>
            <div class="bulletin">
                <h1 class="title">公告与活动</h1>
                <div class="content-wrapper">
                    <p class="content">{{seller.bulletin}}</p>
                </div>
                <ul v-if="seller.supports" class="supports">
                    <li class="support-item" v-for="item in seller.supports">
                       <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
                       <span class="text">{{item.description}}</span>
                    </li>
                </ul>
            </div>
            <split></split>
            <div class="pics">
                <h1 class="title">商家实景</h1>
                <div class="pic-wrapper" v-el:pic-wrapper>
                    <ul class="pic-list" v-el:pic-list>
                        <li class="pic-item" v-for="pic in seller.pics">
                            <img :src="pic" width="120" height="90">
                        </li>
                    </ul>
                </div>
            </div>
            <split></split>
            <div class="info">
                <h1 class="title">商家信息</h1>
                <ul class="info-wrapper">
                    <li class="info-item" v-for="info in seller.infos">{{info}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script type= "text/ecmascript-6">
    import BScroll from 'better-scroll';
    import star from 'components/star/star';
    import split from 'components/split/split';
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                favorite: false
            }
        },
        computed: {
           favoriteText() {
              return this.favorite ? '已收藏' : '未收藏';
           }
        },
        components: {
            star,
            split
        },
        created() {
           this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        },
        ready() {
            this._initScroll();
            this._initPics();
        },
        watch: {
           'seller'() {
              this._initScroll();
              this._initPics();
           }
        },
        methods: {
            toggleFavorite(event) {
               if (!event._constructed) {
                   return;
               }
               this.favorite = !this.favorite;
            },
            _initScroll() {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$els.seller, {
                        click: true
                });
                } else {
                    this.scroll.refresh();
                }
            },
            _initPics() {
                if (this.seller.pics) {
                let picWidth = 120;
                let margin = 0;
                let width = (picWidth + margin) * this.seller.pics.length - margin;
                this.$els.picList.style.width = width + 'px';
                this.$nextTick(() => {
                   if (!this.picScroll) {
                   this.picScroll = new BScroll(this.$els.picWrapper, {
                       scrollX: true,
                       eventPassthrough: 'vertical'
                   })
                   } else {
                      this.picScroll.refresh();
                   }
                });
            }
            }
        }
    }
</script>

<style lang= "stylus" rel="stylesheet/stylus">
    .seller
      position: absolute
      top: 174px
      bottom: 0
      left: 0
      width: 100%
      overflow: hidden
      .overview
        position: relative
        padding: 18px
        .title
          margin-bottom: 8px
          line-height: 14px
          color: rgb(7, 17, 27)
          font-size: 14px
        .desc
          padding-bottom: 8px
          line-height: 14px
          border-bottom: 1px solid rgba(7, 17, 27, 0.1)
          font-size: 0
          .star
            display: inline-block
            margin-right: 8px
          .text
            font-size: 10px
            color: rgb(77, 85, 93)
            line-height: 18px
            margin-right: 12px
            vertical-align: top
        .remark
          display: flex
          width: 100%
          padding: 18px 0
          .block
            flex: 1
            text-align: center
            border-right: 1px solid rgba(7, 17, 27, 0.1)
            &:last-child
              border-right :none
            .title
              font-size: 10px
              color: rgb(147, 153, 159)
              line-hegiht: 10px
            .content
              font-size: 10px
              font-weight: 200
              color: rgb(7, 17, 27)
              line-hight: 24px
              .stress
                font-size: 24px
        .favorite
          position: absolute
          right: 18px
          top: 18px
          text-align: center
          .icon-favorite
            display: block
            color: #d4d6d9
            margin-bottom: 4px
            line-height: 24px
            font-size: 24px
            &.active
              color: rgb(240, 20, 20)
          .text
            line-height: 10px
            font-size: 10px
            color: rgb(77, 85, 93)
      .bulletin
        padding: 18px 18px 0 18px
        .title
          margin-bottom: 8px
          line-height: 14px
          color: rgb(7, 17, 27)
          font-size: 14px
        .content-wrapper
          padding: 8px 12px 16px 12px
          border-bottom: 1px solid rgba(7, 17, 27, 0.1)
          .content
           font-size: 12px
           font-weight: 100
           color: rgb(240, 20, 20)
           line-height: 24px
        .supports
           .support-item
             padding: 16px 12px
             border-bottom: 1px solid rgba(7, 17, 27 ,0.1)
             font-size: 0
             &:last-child
               border-bottom: none
             .icon
               font-size: 12px
               display: inline-block
               width: 16px
               height: 16px
               background-repeat: no-repeat
               background-size: 16px 16px
               &.decrease
                 background-image: url(decrease_2@2x.png)
               &.discount
                 background-image: url(discount_2@2x.png)
               &.guarantee
                 background-image: url(guarantee_2@2x.png)
               &.invoice
                 background-image: url(invoice_2@2x.png)
               &.special
                 background-image: url(special_2@2x.png)
             .text
               font-size: 12px
               font-weight: 200
               color: rgb(7, 17, 27)
               line-height: 16px
               display: inline-block
               vertical-align: top
      .pics
        padding: 18px
        .title
          margin-bottom: 12px
          line-height: 14px
          color: rgb(7, 17, 27)
          font-size: 14px
        .pic-wrapper
            width: 100%
            overflow: hidden
            white-space: nowrap
            .pic-list
              font-size: 0
              .pic-item
                display: inline-block
                width: 120px
                height: 90px
                margin-right: 6px
                &:last-child
                  margin: 0
      .info
        padding: 18px 18px 0 18px
        .title
          padding-bottom: 12px
          border-bottom: 1px solid rgba(7, 17, 27, 0.1)
          line-height: 14px
          color: rgb(7, 17, 27)
          font-size: 14px
        .info-wrapper
          .info-item
            font-size: 12px
            line-height: 200
            color: rgb(7, 17, 27)
            line-height: 16px
            padding: 16px 12px
            border-bottom: 1px solid rgba(7, 17, 27, 0.1)
</style>