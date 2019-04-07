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

## 基础语法

### 渲染视图

#### 数据填充

Vue不建议用户直接操作DOM
##### HTML标签Text值

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

##### HTML标签属性值

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

#### 标签事件

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

#### 事件修饰符

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
> ​          2、修饰符组合使用，顺序无关;
>
> ​          3、`@click`值如果是无参函数，可以忽略。

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

#### 数据的双向绑定

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

> v-show指令只能用于表单元素

#### 实例1：跑马灯效果

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

#### 实例2：简易计算器

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

#### 样式属性绑定

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

### 流程控制

#### v-for指令

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

#### v-if指令

```html
<!-- 视图层 -->
<div id="app">
    <div>
        <label for="name">姓名:<input type="text" v-model="name"></label>
        <label for="age">年龄:<input type="text" v-model="age"></label>
        <input type="submit" value="提交" @click="click">
    </div>

    <!-- 情景1：默认使用 -->
    <!-- <p v-for="(friend,i) in friends">
        <input type="checkbox">
        索引:{{ i }},姓名:{{ friend.name }},年龄:{{ friend.age }}
    </p> -->

    <!-- 情景2：默认key为索引，导致在前面加入元素的时候，checkbox控件选中的始终是索引为key的数据 -->
    <!-- <p v-for="(friend,i) in friends" :key="i">
            <input type="checkbox">
            索引:{{ i }},姓名:{{ friend.name }},年龄:{{ friend.age }}
        </p> -->

    <!-- 情景3：对象唯一，作为key -->
    <p v-for="(friend,i) in friends" :key="friend">
        <input type="checkbox">
        索引:{{ i }},姓名:{{ friend.name }},年龄:{{ friend.age }}
    </p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            name: '',
            age: '',
            friends: [
                { name: '张三', age: 18 },
                { name: '李四', age: 19 },
                { name: '小明', age: 20 },
            ],
        },
        methods: {
            click() {
                // this.friends.push({name:this.name, age:this.age});
                this.friends.unshift({ name: this.name, age: this.age });
            }
        }
    });
</script>
```

> 在组件中，使用v-for指令循环时，使用属性绑定`:key`作为唯一值。

v-if指令和v-show指令

```html

<!-- 视图层 -->
<div id="app">
    <!-- <input type="button" value="切换状态" @click="toggle"> -->
    <input type="button" value="切换状态" @click="flag=!flag">
    <p v-if="flag">这是v-if控制的元素</p>
    <p v-show="flag">这是v-show控制的元素</p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            flag: true
        },
        methods: {
            // toggle() {
            //     this.flag = !this.flag;
            // }
        }
    });
</script>
```

> 区别：
>
> ​		v-if指令：每次都会删除或者穿件元素，切换性能消耗较高；
>
> ​		v-show指令：只是切换元素的`display: block;`和`display: none;`，初始渲染消耗较高。
>
> 总结：如果元素涉及到频繁的切换，不建议使用v-if指令。

## 基础语法应用案例：

```html
<!-- 视图层 -->
<div id="app">
    <div class="panel panel-primary">
        <div class="panel-heading">
            <h3 class="panel-title">通讯录</h3>
        </div>
        <div class="panel-body form-inline">
            <label for="id">ID：<input type="text" name="id" class="form-control" v-model="id"></label>
            <label for="name">姓名：<input type="text" name="name" class="form-control" v-model="name"></label>
            <input type="button" value="添加" class="btn btn-primary" @click="add()">
            <label for="keyword">模糊筛选：<input type="text" name="keyword" class="form-control" v-model="keyword"
                    placeholder="名称关键字"></label>
        </div>
    </div>
    <table class="table table-bordered table-hover table-striped">
        <thead>
            <tr>
                <td>ID</td>
                <td>Name</td>
                <td>CTime</td>
                <td>Operation</td>
            </tr>
        </thead>
        <tbody>
            <tr v-for="frident in search(keyword)" :key="frident">
                <!-- <tr v-for="frident in fridents" :key="frident"> -->
                <td>{{frident.id}}</td>
                <td>{{frident.name}}</td>
                <td>{{frident.ctime | dataFilter}}</td>
                <td>
                    <a href="" @click.prevent="del(frident.id)">删除</a>
                </td>
            </tr>
        </tbody>
    </table>
</div>

<!-- VM层（挂载数据层） -->
<script>
    Vue.filter('dataFilter', function (datestr) {
        var dt = new Date(datestr);
        return dt.toLocaleDateString();
    });
    var vm = new Vue({
        el: '#app',
        data: {
            id: '',
            name: '',
            keyword: '',
            fridents: [
                { id: 1, name: '张三', ctime: new Date() },
                { id: 2, name: '李四', ctime: new Date() },
                { id: 3, name: '小明', ctime: new Date() },
                { id: 4, name: '张小明', ctime: new Date() },
            ]
        },
        methods: {
            add() {
                this.fridents.push({ id: this.id, name: this.name, ctime: new Date() });
                // this.id = '';
                // this.name = '';
                this.id = this.name = '';
            },
            del(id) {
                /* this.fridents.some((item, i) => {
                    if (item.id == id) {
                        this.fridents.splice(i, 1);
                        return true;
                    }
                }); */
                var index = this.fridents.findIndex(item => {
                    if (item.id == id) {
                        return true;
                    }
                });
                this.fridents.splice(index, 1);
            },
            search(k) {
                // var list = [];
                // this.fridents.forEach(frident => {
                //     if(frident.name.indexOf(k) != -1) {
                //         list.push(frident);
                //     }
                // });
                // return list;
                return this.fridents.filter(item => {
                    if (item.name.includes(k)) {
                        return item;
                    }
                });
            },
        }
    });
</script>
```

> `some()`、 `findIndex()`方法的使用。
>
> Vue提供的[按键码]( https://cn.vuejs.org/v2/guide/events.html#按键码) ，除此，还可以自定义按键码，[js 里面的键盘事件对应的键码](https://www.cnblogs.com/wuhua1/p/6686237.html)

## 高级用法



### 过滤器

```html
<!-- 视图层 -->
<div id="app">
    <p>{{ message }}</p>
    <p>{{ message | messageFormat }}</p>
    <p>{{ message | messageFormat2('大雾') }}</p>
    <p>{{ message | messageFormat2('阴天') }}</p>
    <p>{{ message | messageFormat2('大雾') | messageFormat3('后天') }}</p>
    <p>{{ message | messageFormat4('大雾', '后天') }}</p>
    <p>{{ ctime | dateFormat }}</p>
</div>

<div id="app2">
    <p>{{ ctime | dateFormat }}</p>
</div>

<!-- VM层（挂载数据层） -->
<script>
    Vue.filter('messageFormat', function (data) {
        return data.replace('晴天', '雨雪');
    });

    Vue.filter('messageFormat2', function (data, arg) {
        return data.replace('晴天', arg);
    });

    Vue.filter('messageFormat3', function (data, arg) {
        return data.replace('今天', arg);
    });

    Vue.filter('messageFormat3', function (data, arg) {
        return data.replace('今天', arg);
    });

    Vue.filter('messageFormat4', function (data, arg1, arg2) {
        return data.replace('晴天', arg1).replace('今天', arg2);
    });

    // 全局过滤器和私有过滤器
    Vue.filter('dateFormat', function (data) {
        return new Date(data).toLocaleString();
    });

    var vm = new Vue({
        el: '#app',
        data: {
            message: '今天的天气是 晴天！',
            ctime: new Date()
        }
    });

    var vm2 = new Vue({
        el: '#app2',
        data: {
            ctime: new Date()
        },
        filters: {//私有过滤器
            dateFormat(data) {
                return new Date(data).toLocaleDateString();
            }
        }
    });
</script>
```



> 用于常见的文本格式化，用在插件表达式和v-bind表达式中。
>
> 过滤器调用 `{{ 插值变量 | 过滤器名称}}`；
>
> 过滤器定义 `Vue.filter("过滤器名称", function(data){/* 过滤处理 */})
>
> 过滤器方法的第一个参数默认是当前数据，其余的是参数。
>
> 全局过滤器-所有的vm实例共享。

### 键盘修饰符

```html
<!-- 视图层 -->
<div id="app">
    <input type="text" v-model="text" @keyup.enter="submit('enter')">
    <!-- F2按键码 触发  按键码 https://cn.vuejs.org/v2/guide/events.html#按键码 -->
    <input type="text" v-model="text" @keyup.113="submit('113')">
    <!-- F2按键码别名 触发 -->
    <input type="text" v-model="text" @keyup.myF2="submit('myF2')">
</div>

<!-- VM层（挂载数据层） -->
<script>
    // 自定义全局按键修饰符
    Vue.config.keyCodes.myF2 = 113;
    var vm = new Vue({
        el: '#app',
        data: {
            text: ''
        },
        methods: {
            submit(type) {
                console.log(`keyup.${type}输入内容为：${this.text}`);
            }
        }
    });
</script>
```

### 自定义指令

```html
<!-- 视图层 -->
<div id="app">
    <input type="text" v-focus v-color="'green'">
    <h3 v-color="'pink'">{{ message }}</h3>  
    <p v-fontweight="100+200 * 4">{{ message }}</p>  
    <p v-backgroundcolor="'red'">{{ message }}</p>  
</div>

<!-- VM层（挂载数据层） -->
<script>
    Vue.directive('focus', {
        bind(el) {
            console.log('指令的声明周期之 bind');
        },
        inserted(el) {
            el.focus();
            console.log('指令的声明周期之 inserted');
        },
        updated() {
            console.log('指令的声明周期之 updated');
        }
    });
    // 自定义一个 设置字体颜色的 指令
    Vue.directive('color', {
        // 样式只有通过指令绑定给了元素，不管这个元素有没有插到页面中去，这个元素肯定有一个内联样式，肯定会显示到页面。
        bind: function (el, binding) {
            console.log(binding);
            el.style.color = binding.value;
        }
    });
    // 如果在bind和inserted触发相同的行为，二部关心其他钩子函数
    Vue.directive('backgroundcolor', function(el, binding){
        el.style.backgroundColor = binding.value;
    });

    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello World!'
        },
        // 局部指令
        directives: {
        //     focus: {
        //         inserted: function(el) { el.focus();}
        //     },
        fontweight: {
                bind:function(el, binding) {
                    el.style.fontWeight = binding.value;}
            }
        }
    });
</script>
```



| 钩子函数 | 说明                                          | 执行次数   |
| -------- | --------------------------------------------- | ---------- |
| bind     | 每当指令绑定到元素上时，元素还没有插入到DOM中 | 一次       |
| inserted | 插入到DOM中时                                 | 一次       |
| updated  | 当VNode更新的时候，执行updated，              | 可触发多次 |

> 说明：1、`Vue.directive(指令名称（不带v-）, 钩子函数对象);`；
>
> ​           2、钩子函数中，第一个参数是el（原生js对象）；钩子函数第二个参数binding，expression-原始表达式，value-表达式的值，name-指令名称，rawName-指令全名称；
>
> ​           3、样式实现在bind，和js行为相关在inserted实现，防止行为不生效。

### Vue生命周期函数(钩子)

![Vue生命周期](https://cn.vuejs.org/images/lifecycle.png)

* 创建期间的声明周期函数
  - beforeCreate
  - created
* 运行期间的声明周期函数
* 销毁期间的声明周期函数

### 数据请求

### 动画











































