# vue
vue学习笔记

## Hello world

视图层

```html
<!-- 视图层 -->
<div id="app">
    {{ message }}
</div>
```

VM层（挂载数据层）

```javascript
var vm = new Vue({
      el: '#app',
      data: {
          message: 'Hello World!'
    	}
});
```

### 实例1

[我的第一个Vue应用](./HelloWorld.html)

## 渲染视图
### 数据填充
Vue不建议用户直接操作DOM
#### HTML标签text
#### HTML标签属性
#### HTML标签事件

#### v-text指令
#### v-html指令
#### v-bind指令
> 三种渲染方式的区别
#### v-cloak解决插值预显示问题
### 事件绑定v-on指令
### 数据的双向绑定v-model指令 
### 实例1：实现跑马灯
### 实例2：简易计算器