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

18. 支付宝小程序安卓机 windowHeight 拿到的高度不准确，获取容器元素的高度来解决。

19. 微信小程序中，uni-icons 外层不可以使用 text 包裹，否则图标无法显示。

20. 微信小程序中，最好不要使用标签、属性、id选择器，都使用 class 选择器。

21. uni.createInnerAudioContext() H5和小程序音频播放的时机不同，小程序在 onPlay 钩子触发时，就在播放了。H5 onPlay 钩子触发时不一定在播放，需要下载。所以在 onCanplay 钩子中才能正确把握播放的时机。

22. uni.createInnerAudioContext() 微信小程序安卓机不能直接播放请求链接，通过 uni.downloadFile 下载文件，拿到下载的文件链接可正常播放。

23. 微信小程序内嵌的 h5 页面不允许调用微信支付，只能通过原生微信小程序来调起支付。
  - h5 支付调起原生的页面进行支付
  - 支付成功或者失败返回 webview 页面
  - 支付成功通知 webview 页面，更新 src 链接，这样会往 webview push 一个最新的 src 链接
  - 最新的 src 链接中进行 replace 的操作会让 webview 的 h5 路由栈多返回一层（非常奇怪）

24. 微信订阅号无法进行网页授权

25. 小程序中 canvas 层级问题，开启 canvas2d 模式解决。以下是 uCharts 的解决方式：
![image](https://user-images.githubusercontent.com/49627376/209652041-1431c079-457b-4e4a-a5d8-ba6cde72bd70.png)

```vue
<template>
  <qiun-data-charts
    type="column"
    canvasId="random-string"
    :canvas2d="true"
    height="278"
    :animation="false"
    :opts="chartOpts"
    :chartData="chartData"
    :tooltipShow="false"
    :disableScroll="false"
   />
</template>
```
26.微信小程序底部输入框距离键盘的位置可以使用两种方式来兼容
  - 输入框区域使用定位 - 光标距离输入框的距离可以设置 cursor-spacing
  - 监听输入框的高度，手动设置输入框距离页面底部的距离，这种方式要关闭键盘推页面 adjust-position

27.微信小程序滚动穿透问题。
  - 页面中没有滚动容器，可以在根标签设置 catchtouchmove="return" 来阻止滚动。
  - 有滚动容器可以给全局或者局部的 page 标签设置 height: 100vh; overflow: hidden; 设置之后，页面超出会无法滚动。
  
28.支付宝小程序安卓手机关闭小程序，任务列表中小程序的进程还在。扫码打开小程序无法在触发 onLaunch 钩子。

29.小程序分为热启动和冷启动，安卓机会再冷启动之后，在任务列表中多一个小程序的进程。不 kill 掉这个进程。小程序会变成热启动。扫码进入就无法拿到启动参数。

30.支付宝小程序浏览器使用的 UC 内核（Android browser）从 22 年开始不支持 getUserMedia API，需要使用小程序的 api 实现。

## H5

1. 在微信浏览器中，拉取浏览器自带的下拉会和自身做的下拉刷新冲突，导致 android 出现 bug。
    - 为下拉刷新添加 touchcancel 事件，不过会导致难以触发。vant 的实现就是这个。
    - 设置 body 样式 overflow: hidden; 但是这样所有页面的滚动给就要自己控制了。
    
2. 修改文字颜色，除了直接设置 color，还可以通过背景的方式去改变文字颜色
    ```css
    .class {
      background-color: #0093E9;
      color: transparent;
      background-clip: text;
    }
    ```
3. 解决子元素设置 margin-top 影响到父元素
   ```css
   .parent {
     padding-top: 1px; // 设置 1px 会破坏非空白的折叠条件
   }
   .child {
     margin-top: 50px;
   }
   ```
4. 苹果浏览器工具栏在底部，页面高度设置 100vh，会有滚动条出现。100vh 是包含了工具栏的高度。
  - 设置 100% 来代替 100vh
  - 100vh - 工具栏的高度
  
5. 在微信中进行网页授权，使用 replace 跳转到微信授权地址，路由栈中还会存在之前的地址。replace 无效
   
## element-ui

1. 树形懒加载表格的增删改之后，数据状态的[更新方案](https://blog.csdn.net/weixin_39150852/article/details/113727283)
