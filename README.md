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

### 事件修饰符

```html

<!-- 视图层 -->
<div id="app">
    <h4>事件冒泡</h4>
    <div style="width: 300px;height: 90px;background-color: gray;" @click="outerHandler">外部div
        <div style="width: 80%;background-color: yellow;" @click="middleHandler"> 中间div
            <div style="width: 60%;background-color: green;" @click="innerHandler"> 内部div</div>
        </div>
    </div>
    <h4>stop修饰符——阻止冒泡</h4>
    <div style="width: 300px;height: 90px;background-color: gray;" @click="outerHandler">外部div
        <div style="width: 80%;background-color: yellow;" @click.stop="middleHandler"> 中间div
            <div style="width: 60%;background-color: green;" @click="innerHandler"> 内部div</div>
        </div>
    </div>
    <h4>self修饰符——只有自己才会触发事件</h4>
    <div style="width: 300px;height: 90px;background-color: gray;" @click="outerHandler">外部div
        <div style="width: 80%;background-color: yellow;" @click.self="middleHandler"> 中间div
            <div style="width: 60%;background-color: green;" @click="innerHandler"> 内部div</div>
        </div>
    </div>
    <h4>once修饰符——只触发一次</h4>
    <div style="width: 300px;height: 90px;background-color: gray;" @click="outerHandler">外部div
        <div style="width: 80%;background-color: yellow;" @click.once="middleHandler"> 中间div
            <div style="width: 60%;background-color: green;" @click="innerHandler"> 内部div</div>
        </div>
    </div>
    <h4>privent修饰符——阻止默认行为</h4>
    <a href="./01、HelloWorld.html" @click="outerHandler">超链接添加onclick事件</a>
    <a href="./01、HelloWorld.html" @click.prevent="outerHandler">添加privent修饰符</a>
    <h4>capture修饰符——实现事件捕获机制</h4>
    <div style="width: 300px;height: 90px;background-color: gray;" @click.capture="outerHandler">外部div
        <div style="width: 80%;background-color: yellow;" @click.once="middleHandler"> 中间div
            <div style="width: 60%;background-color: green;" @click="innerHandler"> 内部div</div>
        </div>
    </div>
    <h4>修饰符组合使用</h4>
    <a href="./01、HelloWorld.html" @click.prevent.once="outerHandler">添加privent修饰符</a>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        methods: {
            outerHandler() { console.log('点击了外部div'); },
            middleHandler() { console.log('点击了中间div'); },
            innerHandler() { console.log('点击了内部div'); },
        }
    });
</script>
```

> 说明：1、stop修饰符和self修饰符的区别：self修饰符只阻止当前元素触发，不阻止其他元素的事件冒泡；
>
> ​          2、修饰符组合使用，顺序无关。

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

### 样式属性绑定

```html
<!-- 视图层 -->
<div id="app">
    <p>没有样式</p>
    <!-- class样式 -->
    <p class="color bold italic size">普通文本样式</p>
    <p :class="['color', 'bold', 'italic', 'size']">Vue属性绑定数组</p>
    <p :class="['color', 'bold', 'italic', flag ? 'size' : '']">Vue属性绑定数组-使用三元表达式</p>
    <p :class="['color', 'bold', 'italic', {size : flag}]">Vue属性绑定数组-嵌套对象</p>
    <p :class="{color : flag, bold : flag, italic : flag, size : flag}">Vue属性绑定-对象</p>
    <p :class="classObj">Vue属性绑定-对象</p>
    <p :class="">Vue属性绑定-对象1111</p>
    <!-- <p :class="classObj()">Vue属性绑定-对象</p> -->
    <!-- 内联样式 -->
    <p style="color: red;font-weight: bold;">内联样式-普通文本样式</p>
    <p :style="styleObj">Vue属性绑定-内联样式-普通文本样式-对象</p>
    <p :style="[styleObj, style2Obj]">Vue属性绑定-内联样式-普通文本样式-对象数组</p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            flag: true,
            // classObj:{color : this.flag, bold : this.flag, italic : this.flag, size : this.flag},
            classObj: { color: true, bold: true, italic: true, size: true },
            styleObj: { color: 'red', 'font-weight': 'bold' },
            style2Obj: { 'font-size': '200%' },
        },
        methods: {
            // classObj() {
            //     return {color : this.flag, bold : this.flag, italic : this.flag, size : this.flag};
            // }
        }
    });
</script>
```

## 流程控制

### v-for指令

```html
<!-- 视图层 -->
<div id="app">
    <!-- 迭代数组 -->
    <p v-for="hobby in hobbies">{{ hobby }}</p>
    <p v-for="(hobby, key) in hobbies">索引:{{ key }} - {{ hobby }}</p>
    <p v-for="friend in friends">姓名:{{ friend.name }},年龄:{{friend.age}}</p>
    <!-- 迭代对象属性 -->
    <p v-for="item in address">{{item}}</p>
    <p v-for="(item, key) in address">{{key}} : {{item}}</p>
    <p v-for="(item, key, index) in address">索引:{{ index }} - {{key}} : {{item}}</p>
    <!-- 迭代数字 -->
    <p v-for="num in 5">{{num}}</p>
    <p v-for="(num, index) in 5">索引:{{ index }} - {{ num }}</p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            hobbies: ['绘画', '唱歌', '游泳', '篮球'],
            friends: [
                {name:'张三', age:18},
                {name:'李四', age:19},
                {name:'小明', age:20},
            ],
            address: {
                state: 'China',
                province: 'Shanghai',
                city: 'Shanghai',
                street: 'No. xxx Jianguo Middle Road'
            }
        }
    });
</script>
```

> 说明：1、遍历数组，遍历项从左向右依次是值（value），索引(index)；遍历对象，遍历项从左向右依次是值（value），键（key），索引(index)；
>
> ​            2、迭代数字是从1开始的。

### v-if指令

