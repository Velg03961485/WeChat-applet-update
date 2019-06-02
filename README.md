# WeChat-applet-update
(ｷ｀ﾟДﾟ´)!!微信小程序更新机制（强制更新-添加即可使用）


# 微信更新介绍

1.微信本身自带更新属性，不过对微信版本有要求，并且是在发布之后的24小时以后


2.强制更新机制（代码实现）


## 更新模块代码


```
getNre(){
    let _this = this;
    // 检测版本问题，让用户进入更新，防止缓存
    if (wx.canIUse('getUpdateManager')) { // 基础库 1.9.90 开始支持，低版本需做兼容处理
      const updateManager = wx.getUpdateManager();
      updateManager.onCheckForUpdate(function (result) {
        console.log(result)
        
        if (result.hasUpdate) { // 有新版本
          

          updateManager.onUpdateReady(function () { // 新的版本已经下载好
            wx.showModal({
              title: '更新提示',
              content: '新版本已经下载好，请重启应用。',
              success: function (result) {
                if (result.confirm) { // 点击确定，调用 applyUpdate 应用新版本并重启
                  updateManager.applyUpdate();
                  _this.cheekaccessToken();
                }else{
                  wx.showToast({
                    title: '您使用的是老版本，将影响体验',
                    icon: 'none',
                    duration: 1000//持续的时间
                  })
                  _this.cheekaccessToken();
                }
              }
            });
          });
          updateManager.onUpdateFailed(function () { // 新的版本下载失败
            wx.showModal({
              title: '已经有新版本了哟~',
              content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~'
            });
          });
        }else{
          _this.cheekaccessToken();
        }
      });
    } else { // 有更新肯定要用户使用新版本，对不支持的低版本客户端提示
      wx.showModal({
        title: '温馨提示',
        content: '当前微信版本过低，无法使用该应用，请升级到最新微信版本后重试。'
      });
    }
  },
  ```

  ## 使用方式


  可以直接复制此函数到更新页面的逻辑中，然后在onShow中调用


  ## 模拟更新

  在开发者工具编译栏，下拉-添加编译模式（创建一个新的模式）-启动页路径（你的第一页面的路径-带有更新代码的）-勾选下次编译模拟更新


  如果你还想进行再次模拟，在你新加的编译模式后，有一个修改，然后再勾选下次编译模拟更新



  ## 附

  有开发者，坚持要把这块更新代码放大app.js onLaunch中，在整个程序加载的时候，自主触发函数


  官方介绍，页面的生命周期是小于app.js中的生命周期


  其实不然，在第一页面触发的生命周期分明比app.js中的onLaunch先执行，然后又执行一遍页面的生命周期


  根据使用场景来说，如果放在onLaunch中，你要到页面级别去调用更新机制，发现根本不起作用（哪些说更新只能放在onLaunch中才能有效的，是在不明白）


  甚至出了注意，采用回调函数0.0，这个方式我试过，感觉吧，不要用想不开的方式去处理


  




