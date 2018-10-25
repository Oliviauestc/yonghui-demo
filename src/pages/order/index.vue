<template>
  <div class="container">
    订单页面
  </div>
</template>

<script>
export default {
  data () {
    return {
      current: 'homepage',
      userInfo: ''
    }
  },
  methods: {
    getUserInfo () {
      wx.getSetting({
        success: function (res) {
          if (res.authSetting['scope.userInfo']) {
            // 已经授权，可以直接调用 getUserInfo 获取头像昵称
            wx.getUserInfo({
              success: function (res) {
                console.log(res.userInfo)
              }
            })
          }
        }
      })
    //   // 调用登录接口
    //   wx.login({
    //     success: () => {
    //       wx.getUserInfo({
    //         success: (res) => {
    //           console.log('用户信息', res.userInfo)
    //           this.userInfo = res.userInfo
    //         }
    //       })
    //     }
    //   })
    }
  },
  created () {
    console.log('用户信息', this.userInfo)
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
    console.log('用户信息', this.userInfo)
  }
}
</script>

<style scoped>
.container {
  /*border:1px solid red;*/
}
.bottom_nav {
  /*border:1px solid red;*/
  width:100%;
}
</style>
