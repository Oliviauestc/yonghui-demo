<template>
  <div class="cart-wrap">
    <div class="empty-cart" v-if="items.length === 0">
      <img :src="emptyCart">
      <div class="add-text">不剁手的日子好空虚</div>
      <div class="to-main" @tap="handleChange">去首页逛逛</div>
    </div>
    <div v-else class="notempty-cart">
      <div class="choose-all">
        <input type="checkbox">
        <span class="choose-all_right">满18元免运费</span>
      </div>
      <div v-for="(item, index) in items" :key="index" class="goods-item">
        <div class="goods-select">
          <input type="checkbox" @tap="handleBuyList(index)">
        </div>
        <div class="goods-img">
          <img :src="item.img">
        </div>
        <div class="goods-detail">
          <div>{{item.goodsName}}</div>
          <div class="goods-detail_bottom">
            <span class="special-price">￥{{item.price}}</span>
            <span class="button-group">
              <span class="add-icon" @tap="addToCart(index)">+</span>
              <span>{{item.count}}</span>
              <span class="add-icon" @tap="moveOut(index)">-</span>
            </span>
          </div>
        </div>
      </div>
      <div class="sum-money">
        <div class="show-money">
          <div>
            <span>商品合计:</span>
            <span class="text">￥{{totalPrice}}</span>
          </div>
          <p>运费{{totalPrice >= 18 ? 0 : 6}}元</p>
        </div>
        <div class="buy-button" @tap="toBuy">
          去结算
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import emptyCart from '../../images/empty_cart.png'
export default {
  data () {
    return {
      emptyCart,
      totalPrice: 0,
      selectedItems: [],
      items: [],
      data: [
        {
          key: 1,
          value: '选项1',
          disabled: false
        },
        {
          key: 2,
          value: '选项2',
          disabled: false
        },
        {
          key: 3,
          value: '选项3',
          disabled: false
        },
        {
          key: 4,
          value: '选项4',
          disabled: false
        }
      ],
      value1: [1, 4]
    }
  },
  methods: {
    addToCart (index) {
      console.log('@@@添加之前***', this.items[index].count)
      var item = this.items[index]
      item.count++
      this.items.splice(index, 1, item)
      console.log('@@@添加之后***', this.items[index].count)
      this.sumPrice()
    },
    moveOut (index) {
      console.log('@@@减少之前', this.items[index])
      this.$store.dispatch('MoveOut', this.items[index])
      this.items = this.$store.state.cart.cartItems
      console.log('@@@减少之后', this.items)
      this.sumPrice()
    },
    handleBuyList (index) {
      if (this.items[index].isChecked === false || this.items[index].isChecked === undefined) {
        console.log('add to buy', index)
        this.selectedItems.push(this.items[index])
      } else {
        console.log('delete what', this.items[index].goodsName)
        var id = this.items[index].id
        for (var i = 0; i < this.selectedItems.length; i++) {
          if (this.selectedItems[i].id === id && this.selectedItems[i].isChecked === true) {
            this.selectedItems.splice(i, 1)
            break
          }
        }
      }
      this.items[index].isChecked = !this.items[index].isChecked
      this.sumPrice()
    },
    sumPrice () {
      console.log('计算总价', this.selectedItems)
      this.totalPrice = 0
      for (let i = 0; i < this.selectedItems.length; i++) {
        this.totalPrice = this.totalPrice + this.selectedItems[i].count * this.selectedItems[i].price
      }
    },
    handleChange () {
      wx.switchTab({
        url: '/pages/index/main'
      })
    },
    toBuy () {
      wx.navigateTo({
        url: '/pages/order/main'
      })
    }
  },
  created () {
    this.items = this.$store.state.cart.cartItems
    this.items.forEach((item, index) => {
      this.$set(this.items[index], 'isChecked', false)
    })
  },
  beforeUpdate () {
    console.log('调用了beforeUpdate钩子函数')
  },
  updated () {
    console.log('调用了updated钩子函数')
  }
}
</script>

<style>
.cart-wrap {
  display: flex;
  height: 100%;
}
.empty-cart {
  width: 100%;
  height: 500px;
  text-align: center;
}
.empty-cart img{
  width: 300px;
  height: 300px;
}
.empty-cart .add-text{
  margin-bottom: 30px;
}
.empty-cart .to-main{
  width: 150px;
  margin: 0 auto;
  height: 30px;
  border: 1px solid #ff6633;
  background-color: #ffffff;
  line-height: 30px;
  color: #ff6633;
}
.notempty-cart {
  width: 100%;
  /*padding: 15px 15px 0px 15px;*/
}
.choose-all {
  border-bottom: 1px solid #cccccc;
  padding: 15px 15px 15px 15px;
  margin: 15px 0px;
}
.choose-all_right {
  float: right;
  color: #ff6633;
}
.goods-item {
  height: 100px;
  border-bottom: 1px solid #cccccc;
  padding: 15px 15px 15px 15px;
  margin-bottom: 20px;
}
.goods-select {
  display: inline-block;
  width: 10%;
  height: 100%;
  float: left;
  text-align: center;
  vertical-align: middle;
}
.goods-select input {
  margin-top:50%;
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
  width: 45%;
  height: 100%;
  position: relative;
}
.goods-detail_bottom {
  font-size:20px;
  width: 100%;
  position: absolute;
  bottom: 5px;
  color: #ff6633;
}
.button-group {
  float: right;
  margin-right: 10px;
}
.add-icon {
  display: inline-block;
  width: 30px;
  height: 30px;
  background-color: #ff6633;
  border-radius: 15px;
  text-align: center;
  vertical-align: middle;
  vertical-align: middle;
  line-height: 30px;
  color: #ffffff;
}
.sum-money {
  height: 50px;
  border-bottom: 1px solid #cccccc;
  padding: 15px 15px 15px 15px;
}
.show-money {
  float: left;
  width: 70%;
}
.show-money .text {
  font-size: 20px;
  font-weight: 700;
  color: #ff6633;
}
.buy-button {
  text-align: center;
  margin-left: 70%;
  width: 25%;
  height: 25px;
  border: 1px solid #ff6633;
  border-radius: 5px;
  background-color: #ff6633;
  color: #ffffff;
}
</style>
