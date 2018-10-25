<template>
  <div class="class-wrap">
    <div class="nav-left">
      <ul>
        <li v-for="(item, index) in list" :key="index" class="nav-item" :class="curNav == item.id ? 'active' : ''" @tap="selectClass" :data-id="item.id" :data-index="index">{{item.value}}</li>
      </ul>
    </div>
    <div class="main-container">
      <div v-for="(item, index) in curClass" :key="index" class="goods-item">
        <div class="goods-img">
          <img :src="item.img">
        </div>
        <div class="goods-detail">
          <div>{{item.goodsName}}</div>
          <div class="goods-detail_bottom">
            <span class="special-price">￥{{item.price}}</span>
            <span class="add-icon" @tap="addToCart(index)">+</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Use Vuex
// import store from './store'

export default {
  data () {
    return {
      curNav: 1,
      curIndex: 0,
      list: [
        {
          id: 1,
          value: '月饼'
        },
        {
          id: 2,
          value: '新鲜蔬果'
        },
        {
          id: 3,
          value: '家禽海鲜'
        },
        {
          id: 4,
          value: '蛋类'
        },
        {
          id: 5,
          value: '日配冷藏'
        },
        {
          id: 6,
          value: '粮油调味'
        },
        {
          id: 7,
          value: '熟食面包'
        },
        {
          id: 8,
          value: '酒水饮料'
        },
        {
          id: 9,
          value: '休闲零食'
        },
        {
          id: 10,
          value: '个护化妆'
        },
        {
          id: 11,
          value: '母婴用品'
        }
      ],
      curClass: [],
      allClass: [
        [
          {
            id: 1,
            goodsName: '五仁月饼',
            img: '../../static/images/wuren.jpg',
            price: 199
          },
          {
            id: 2,
            goodsName: '莲蓉月饼',
            img: '../../static/images/lianrong.jpg',
            price: 199
          },
          {
            id: 3,
            goodsName: '流心月饼',
            img: '../../static/images/cake.jpg',
            price: 299
          },
          {
            id: 8,
            goodsName: '冰皮月饼',
            img: '../../static/images/bingpi.jpg',
            price: 99
          },
          {
            id: 9,
            goodsName: '抹茶月饼',
            img: '../../static/images/mocha.jpg',
            price: 259
          }
        ],
        [
          {
            id: 4,
            goodsName: '红心猕猴桃1个',
            img: '../../static/images/tao.jpg',
            price: 6
          },
          {
            id: 5,
            goodsName: '牛油果1个',
            img: '../../static/images/niu.jpg',
            price: 8
          },
          {
            id: 6,
            goodsName: '莲雾1个',
            img: '../../static/images/lian.jpg',
            price: 5
          },
          {
            id: 7,
            goodsName: '葡萄500g',
            img: '../../static/images/putao.jpg',
            price: 12.9
          },
          {
            id: 10,
            goodsName: '芒果500g',
            img: '../../static/images/mango.jpg',
            price: 25.5
          }
        ]
      ]
    }
  },
  computed: {
    // count () {
    //   return this.$store.state.cart.cartItems
    // }
  },
  methods: {
    selectClass (e) {
      let id = e.target.dataset.id
      let index = parseInt(e.target.dataset.index)
      this.curNav = id
      this.curIndex = index
      this.curClass = this.allClass[id - 1]
    },
    addToCart (index) {
      console.log('@@@', this.allClass[this.curIndex][index])
      this.$store.dispatch('Add', this.allClass[this.curIndex][index])
    }
  },
  created () {
    this.curClass = this.allClass[0]
  }
}
</script>

<style>
.class-wrap {
  display: flex;
  height: 100%;
}
.nav-left {
  overflow:auto;
  height: 100%;
  flex: 0 1 25%;
}
.main-container {
  height: 100%;
  overflow:auto;
  flex: 1 1 auto;
  /*border:1px solid yellow;*/
  padding-top: 10px;
  padding-left: 5%;
}
.nav-item {
  border:1px solid #D0D0D0;
  border-top:0px;
  background-color: #F8F8F8;
  height: 50px;
  display:flex;
  flex-direction:column;
  align-items:center;/*垂直居中*/
  justify-content: center;/*水平居中*/
}
.active {
  background-color: #ffffff;
  border-right:0px;
}
.goods-item {
  height: 100px;
  border-bottom: 1px solid #cccccc;
  padding-bottom: 10px;
  margin-bottom: 20px;
}
.goods-img {
  display: inline-block;
  width: 40%;
  height: 100%;
}
.goods-img img {
  width: 100%;
  height: 100%;
}
.goods-detail {
  display: inline-block;
  float: right;
  width: 50%;
  height: 100%;
  position: relative;
}
.goods-detail_bottom {
  width: 100%;
  position: absolute;
  bottom: 5px;
  color: #ff6633;
}
.add-icon {
  display: inline-block;
  width: 20px;
  height: 20px;
  background-color: #ff6633;
  border-radius: 10px;
  text-align: center;
  vertical-align: middle;
  line-height: 20px;
  color: #ffffff;
  position: absolute;
  right: 5px;
}
</style>
