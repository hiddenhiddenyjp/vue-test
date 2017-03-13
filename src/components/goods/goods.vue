<template>
  <div class="goods">
    <div class="menu-wrapper" v-el:menu-wrapper>
        <ul>
            <li v-for="item in goods" class="menu-item" :class="{'current':currentIndex===$index}" @click="selectMenu($index,$event)">
                <span class="text">
                    <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
                </span>
            </li>
        </ul>
    </div>
    <div class="foods-wrapper" v-el:foods-wrapper>
        <ul>
            <li v-for="item in goods" class="food-list food-list-hook">
                <h1 class="title">{{item.name}}</h1>
                <ul>
                    <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item">
                        <div class="icon">
                            <img :src="food.icon" alt="" width="57" height="57">
                        </div>
                        <div class="content">
                            <h2 class="name">{{food.name}}</h2>
                            <p class="desc">{{food.description}}</p>
                            <div class="extra">
                                <span class="count">月售{{food.sellCount}}份</span>
                                <span>好评率{{food.rating}}%</span>
                            </div>
                            <div class="price">
                                <span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </div>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
    <shopcart v-ref:shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice">
    </shopcart>
  </div>
    <food :food="selectedFood" v-ref:food></food>
</template>
<script type= "text/ecmascript-6">
    import BScroll from 'better-scroll';
    import shopcart from 'components/shopcart/shopcart';
    import cartcontrol from 'components/cartcontrol/cartcontrol'
    import food from 'components/food/food';
    const ERR_OK = 0;
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                goods: [],
                listHeight: [],
                scrollY: 0,
                selectedFood: {}
            }
        },
        computed: {
            currentIndex() {
                for (let i = 0; i < this.listHeight.length; i++) {
                    let height1 = this.listHeight[i];
                    let height2 = this.listHeight[i + 1];
                    if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                        return i;
                    }
                }
                return 0;
            },
            selectFoods() {
                let foods = [];
                this.goods.forEach((good) => {
                    good.foods.forEach((food) => {
                        if (food.count) {
                            foods.push(food);
                        }
                    });
                });
                return foods;
            }
        },
        created() {
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
            this.$http.get('api/goods').then((response) => {
                response = response.body;
                if (response.errno === ERR_OK) {
                    this.goods = response.data;
                    this.$nextTick(() => {
                    this._initScroll();
                    this._calculateHeight();
                });
                }
            });
        },
        components: {
           shopcart,
           cartcontrol,
           food
        },
        events: {
           'cart.add'(target) {
              this._drop(target);
           }
        },
        methods: {
            selectMenu(index, event) {
                if (!event._constructed) {
                     return;
                }
                let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
                let el = foodList[index];
                this.foodsScroll.scrollToElement(el, 300);
            },
            selectFood (food, event) {
                if (!event._constructed) {
                    return;
                }
                this.selectedFood = food;
                this.$refs.food.show();
            },
            _initScroll() {
                this.menuScroll = new BScroll(this.$els.menuWrapper, {
                    click: true
                });
                this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
                    click: true,
                    probeType: 3
                });
                this.foodsScroll.on('scroll', (pos) => {
                    this.scrollY = Math.abs(Math.round(pos.y));
                })
            },
            _drop(target) {
                this.$refs.shopcart.drop(target);
            },
            _calculateHeight() {
                let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
                let height = 0;
                this.listHeight.push(height);
                for (let i = 0; i < foodList.length; i++) {
                    let item = foodList[i];
                    height += item.clientHeight;
                    this.listHeight.push(height);
                }
            }
        }
    };
</script>

<style lang= "stylus" rel="stylesheet/stylus">
    .goods
      display: flex
      position: absolute
      width: 100%
      top: 179px
      bottom: 46px
      overflow: hidden
      .menu-wrapper
        flex: 0 0 80px
        width: 80px
        background: #f3f5f7
        .menu-item
          display: table
          height: 54px
          width: 56px
          padding:0 12px
          line-height: 14px
          &.current
            position: relative
            z-index: 10
            margin-top: -1px
            background: #fff
            font-weight: 700
            .text
              border: none
          .text
            display: table-cell
            font-size: 12px
            font-weight: 200
            line-height: 14px
            vertical-align: middle
            border-bottom: 1px solid rgba(7,17,27,0.1)
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
      .foods-wrapper
        flex:1
        .food-list
          .title
            padding-left:14px
            border-left:2px solid #d9dde1
            font-size: 12px
            color: rgb(147,153,159)
            line-height: 26px
            height: 26px
            background: #f3f5f7
          .food-item
            display: flex
            margin: 18px
            &:last-child
              margin-bottom: 0
            .icon
               flex: 0 0 57px
               margin-right: 10px
            .content
               flex: 1
               position: relative
               .name
                 font-size: 14px
                 color: rgb(7,17,27)
                 line-height: 14px
                 margin:2px 0 8px 0
               .desc, .extra
                 font-size: 10px
                 color: rgb(147,153,159)
                 margin: 8px 0
                 line-height: 12px
               .extra
                  .count
                     margin-right: 12px
               .price
                  .now
                    font-size: 14px
                    color: rgb(240,20,20)
                    font-weight: 700
                    line-height: 24px
                    margin-right:8px
                  .old
                    font-size: 10px
                    color: rgb(147,153,159)   
               .cartcontrol-wrapper
                 position: absolute
                 right: 0
                 bottom: 0
</style>