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
