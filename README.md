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
#### HTML标签Text值

```html
<!-- 视图层 -->
<div id="app">
    <!-- 普通插值 -->
    <p>插值普通字符串变量：<span>内容前缀-{{ message }}-内容后缀</span></p>
    <p>插值HTML片段：<span>内容前缀-{{ html }}-内容后缀</span></p>
    <p>插值函数返回值：<span>内容前缀-{{ datastring('你好') }}-内容后缀</span></p>
    <!-- v-text指令 -->
    <p>v-text指令渲染普通字符串变量：<span v-text="message">原内容会被覆盖掉</span></p>
    <p>v-text指令渲染HTML片段：<span v-text="html">原内容会被覆盖掉</span></p>
    <p>v-text指令渲染函数返回值：<span v-text="datastring('你好')">原内容会被覆盖掉</span></p>
    <!-- v-html -->
    <p>v-html指令渲染HTML片段：<span v-html="html">原内容会被覆盖掉</span></p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: '消息内容',
            html: '<strong>HTML片段</strong>',
        },
        methods: {
            datastring(greeting) {
                return greeting + ', 今天是' + new Date();
            }
        }
    });
</script>
```

> 说明：1、v-html指令用来执行并显示HTML片段；v-text指令原样显示字符串内容。
>
> ​           2、v-html指令和v-text指令覆盖标签原有内容，普通插值`{{ var }}`可以拼接显示。

#### HTML标签属性值

```html
<!-- 视图层 -->
<div id="app">
    <!-- 标签属性值 -->
    <input type="button" v-bind:value="title">
    <!-- 简写 -->
    <input type="button" :value="title">
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            title: '按钮标题',
        }
    });
</script>
```

> 说明：v-bind指令简写为`:`。

#### HTML标签事件

```html
<!-- 视图层 -->
<div id="app">
    <!-- 标签事件-->
    <input type="button" value="按钮" v-on:click="click">
    <!-- 简写 -->
    <input type="button" value="按钮" @click="click">
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            click() {
                alert('点击了我 ~');
            },
        }
    });
</script>
```

> 说明：v-on指令简写为`@`。

#### v-cloak解决插值预显示问题

```css
[v-cloak] {
    display: none;
}
```

```html
<!-- 视图层 -->
<div id="app" v-cloak>
    {{ message }}
</div>

<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello World!'
        }
    });
</script>
```

### 数据的双向绑定

```html
<!-- 视图层 -->
<div id="app">
    <p><span>{{ message }}</span></p>
    <label for="content">输入：</label>
    <input type="text" name="content" v-model="message">
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello World!'
        }
    });
</script>
```

### 实例1：跑马灯效果

```html
<!-- 视图层 -->
<div id="app">
    <p>{{ message }}</p>
    <input type="button" value="开始" @click="start">
    <input type="button" value="结束" @click="stop">
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello World!',
            interval: null
        },
        methods: {
            start() {
                if (this.interval == null) {
                    this.interval = setInterval(() => {
                        var str1 = this.message.substring(0, 1);
                        var str2 = this.message.substring(1);
                        this.message = str2 + str1;
                    }, 300);
                }
            },
            stop() {
                clearInterval(this.interval);
                this.interval = null;
            },
        }
    });
</script>
```

### 实例2：简易计算器

```html

<!-- 视图层 -->
<div id="app">
    <input type="text" name="num1" v-model="num1">
    <select name="operator" v-model="operator">
        <option value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
    </select>
    <input type="text" name="num2" v-model="num2">
    <input type="button" value="=" @click="click">
    <input type="text" name="num3" v-model="num3">
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            num1: 0,
            num2: 0,
            num3: '',
            operator: '+'
        },
        methods: {
            click() {
                if ('+' === this.operator) {
                    this.num3 = parseInt(this.num1) + parseInt(this.num2);
                } else if ('-' === this.operator) {
                    this.num3 = parseInt(this.num1) - parseInt(this.num2);
                } else if ('*' === this.operator) {
                    this.num3 = parseInt(this.num1) * parseInt(this.num2);
                } else {
                    this.num3 = parseInt(this.num1) / parseInt(this.num2);
                }
            }
        }
    });
</script>
```

