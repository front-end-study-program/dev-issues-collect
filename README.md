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
