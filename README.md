# dev-issues-collect
开发遇到的疑难杂症收集

## uni-app
1. v-show 在组件上使用不生效，无法继承到组件内部根元素上。通过在外层包一层view来做显示隐藏。
2. 输入框软键盘唤起导致页面整体上移问题<br/>
  2.1 支付宝小程序暂不支持<br/>
  2.2 微信小程序可以为输入框设置 adjust-position 属性
3. 支付宝小程序输入框光标错位问题<br/>
  3.1 可以设置 enableNative=false 属性来解决问题。但是 confirm-type 属性设置会失效<br/>
  3.2 给输入框或者父级元素设置 position: fixed; css属性<br/>
4. 属性传值会把函数类型的值转递消失。如：JSON.stringify 一般 [解决方案](https://github.com/dcloudio/uni-app/issues/1522) 重写 uniapp 挂载在 vue 实例属性上的[__patch__](https://github1s.com/dcloudio/uni-app/blob/HEAD/packages/vue-cli-plugin-uni/packages/mp-vue/dist/mp.runtime.esm.js#L5428) 方法
5. 输入框类型为 number 的时候，设置 maxlength 无用属性会导致事件无法触发，绑定的值无法改变
6. 支付宝小程序有提供 contact-button 智能客服的界面可直接套用
7. scroll-view 滚动到底部，可以设置 scroll-into-view="id"，需要重置才会重新触发到底
8. pages.json 中 pages 第一项不是 tarbar 的路由页面会导致 IOS 中 tarbar 向上错位
9. 支付宝小程序调用录音 API 生成的临时文件就是以 .audio 为后缀。不会因为设置 format 值而改为 .mp3。通过 uploadFile 上传之后会变成 wav 格式的。如果需要是 mp3 格式可通过重写文件改后缀的方式。
10. 支付宝小程序 readFileSync 写入的文件是永久的
11. 支付宝小程序 uploadFile 接口会默认设置 Content-type 为 'multipart/form-data'，如果我们再次在 header 设置一遍可能会导致 android 上传报系统异常的问题。
12. 支付宝小程序中遍历渲染数组，key 属性如果是一致的话，会导致事件转递的值为 undefined。
13. 支付宝实现自定义导航栏。支付宝自带的导航栏不支持渐变色。文字颜色不能随意改变
  - pages.json
  
    ```json
      "style": {
        "navigationBarTitleText": "标题",
        "transparentTitle": "always",
        "navigationBarTextStyle": "white",
        "navigationBarBackgroundColor": "black"
      }
    ```
  - 自定义导航栏
  
    ```vue
    <template>
      <view class="nav-bar" :style="{height: titleBarHeight + statusBarHeight}"></view>
    </template>
    <script>
    const { titleBarHeight, statusBarHeight } = uni.getSystemInfoSync();
    export default {
      name: 'NavBar',
      data() {
        return {
          titleBarHeight, // 手机标题栏高度
          statusBarHeight // 手机状态栏的高度
        }
      }
    }
    </script>
    ```
 
14. 支付宝小程序[schema链接](https://opendocs.alipay.com/support/01rb18)默认是跳转发布上线的应用，如果需要跳转体验版或者开发版本[文档](https://opendocs.alipay.com/support/01rb0j)。需要开启对应版本中右上角 -> 联调设置 -> 联调扫码版本.

15. 支付宝小程序禁止页面回弹
    ```json
    // page.json
    {
      "pages": [
        "path": "path",
        "style": {
          "allowsBounceVertical": "NO"
        }
      ]
    }
    ```
16. vite 打包 h5 代码，代码中不能有解构 uni 变量的操作，内部构建会将其进行 tree shaking 操作。

17. 支付小程序中文字上下居中老是偏上，可以设置 line-height: normal; 来解决

## H5

1. 在微信浏览器中，拉取浏览器自带的下拉会和自身做的下拉刷新冲突，导致 android 出现 bug。
    - 为下拉刷新添加 touchcancel 事件，不过会导致难以触发。vant 的实现就是这个。
    - 设置 body 样式 overflow: hidden; 但是这样所有页面的滚动给就要自己控制了。
