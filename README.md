# dev-issues-collect
开发遇到的疑难杂症收集

## uni-app
 1. v-show 在组件上使用不生效，无法继承到组件内部根元素上。通过在外层包一层view来做显示隐藏。
 2. 输入框软键盘唤起导致页面整体上移问题<br/>
  2.1 支付宝小程序暂不支持<br/>
  2.2 微信小程序可以为输入框设置 adjust-position 属性
 3. 支付宝小程序输入框光标错位，可以设置 enableNative=false 属性来解决问题
