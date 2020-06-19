# 获取用户信息的组件
> 微信脑子瓦特的设计，`<button open-type="getUserInfo">`主动点击后拿到加密信息，而获取解密`session_key`所需的`code`，只能在点击之前获取，否则会有很小的几率 `pad block corrupted`。点击之前获取的`code`又会存在超时的问题，重新调用`wx.login`获取`code`又陷入第一个问题，因此设计成弹框类型，弹框时会调用`wx.login`获取新`code`，以保证解密的正确性。

> 由于微信小程序审核机制的限制，进入小程序时强制要求登录的操作有可能不能通过审核，故设计成组件而非页面，仅在必要时调用，已登录状态则直接回调`success`, 未登录时弹框授权。

![avatar](./getUserInfo组件/WX20200620-010204@2x.png ':size=375x690')

## 目录结构
```
login
    |-helper.js 需要开发者完善的文件，补充isLogin判断条件 && 注册方法
    |-index.js
    |-index.json
    |-index.wxml
    |-index.wxss
```

## helper.js
```javascript
export default {
    /*
    * 用户登录状态的判断条件
    * resolve({ success: true })  // 已登录回调success
    * resolve({ success: false })  // 为登录，走授权流程
    */
    isLogin(){
        return new Promise((resolve, reject) => {
            /**
             * checkSession && 验证本地储存的信息
             * 
             */

            resolve({
                success: false
            })
        })
    },

    /*
    * 注册
    * data中包含加密信息、获取解密session_key所需的code
    * 将data发送给服务端，根据返回信息，调用cb回调
    */
    signUp(data, cb){
        /**
         * data {
         *      code,
         *      encrypted_data,
         *      iv
         * }
         * 
         * 自定义接口
         * 例如：userSignUp(data, res => {
         *          if(res.success){ // 保存返回的用户信息，以供isLogin验证使用
         *             cb(true) 
         *          }else{
         *              cb(false, res.msg)
         *          }
         * 
         *      })
         * 
         */
        cb(true)
    }
}
```

## 使用

```
// app.json中全局引入组件
"usingComponents": {
    "login": "./components/sx-components/login/index"
}

// 在需要用的登录的页面，引入组件
<view>
    <view bindtap="search" class="button">查找</view>
</view>
<login />

//调用登录方法
import helper from '../components/sx-components/login/helper';
Page({
   search(){
        helper.login({
            success(){
                console.log('成功')
            },
            fail(e){
                console.log(e)
            }
        })
    } 
})
```

