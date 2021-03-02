## 第一章
### 1.2初学编写hello world
``` js
<script>
    Vue.createApp({
        data() {
            return {
                content: 1
            }
        },
        mounted() {
            //页面加载完成之后执行
            console.log("mounted")
            setInterval(() => {
                this.content += 1;
            }, 1000)
        },
        template: '<div>{{content}}</div>'
    }).mount('#root');
</script>
```
- `createApp().mount();`
> 创建实例，绑定id

- `mounted()`
> 页面加载完成后执行的事情

- `template:`
> 模板 {{变量}}

### 1.3编写字符串反转和内容显示隐藏

> 内容显示隐藏
``` js
<script>
    Vue.createApp({
        data() {
            return {
                show:true,
                content: 'hello world'
            }
        },
        methods:{
            handleBtnClick(){
                this.show = !this.show;
            }
        },
        template: 
        `
        <div>
           <span v-if="show"> {{content}}</span>
            <button v-on:click="handleBtnClick">显示/隐藏</button>
        </div>
        `
        
    }).mount('#root');
</script>
```
---

> 字符串反转
```js
<script>
    Vue.createApp({
        data() {
            return {
                content: 'hello world'
            }
        },
        methods:{
            handleBtnClick(){
                //split('')打散
                //reverse()重新排序
                //join()聚合
                const newContent=this.content.split('').reverse().join('')
                this.content=newContent;
            }
        },
        template: 
        `
        <div>
            {{content}}
            <button v-on:click="handleBtnClick">反转</button>
        </div>
        `
        
    }).mount('#root');
</script>
```
### 1-4 编写TodoList 小功能，了解循环和双向绑定
``` js
<script>
    Vue.createApp({
        data() {
            return {
                inputValue:'',
                list:[]
            }
        },
        methods:{
            handleAddItem(){
                this.list.push(this.inputValue);
                this.inputValue='';
            }
        },
        template: 
        `
        <div>
            <input v-model="inputValue"/>
             <button
             v-on:click="handleAddItem"
             v-bind:title="inputValue"
             >增加</button>
        <ul>
            <li v-for="item of list">{{item}} test</li>
            <li v-for="(item,index) of list">{{item}}  {{index}} test</li>
        </ul>
        </div>
        `
        
    }).mount('#root');
</script>
```
> `v-bind:属性='变量'` 可以在标签内部绑定变量，外部直接用{{变量}}

> `v-for= "item of list" ` 将list中的元素循环到item中

>`v-for="(item,index) of list "` 将索引和元素循环到item中

>`v-model="inputValue"` 和`<input/>`控件双向绑定，变量`inputValue`改变，控件中的数值也会发生改变。

>`this.list.push(this.inputValue);`向`list`数组添加元素

### 1.5组件概念初探todolist组件代码拆分
> 组件就是网页的一部分

``` js
<script>
  const app =   Vue.createApp({
        data() {
            return {
                inputValue:'',
                list:[]
            }
        },
        methods:{
            handleAddItem(){
                this.list.push(this.inputValue);
                this.inputValue='';
            }
        },
        template: 
        `
        <div>
            <input v-model="inputValue"/>
            <button
             v-on:click="handleAddItem"
             v-bind:title="inputValue"
             >增加</button>

        <ul>
            // 下面app定义的组件 todo-item
            <todo-item 
            v-for="(item,index) of list" 
            v-bind:content="item"
            v-bind:index="index" />
            
        </ul>
        </div>
        `
    });
    app.component("todo-item",{
        props:['content','index'],
        template:'<li>{{index}}--{{content}}</li>'
    });
    app.mount("#root");
</script>
```
> `const app = Vue.creatApp()` 创建Vue实例

> ``` js
> app.component('组件名称',{
>      props:['组件属性1'，'组件属性2'],
>      template:'组件内容'
>      });
> ```

> `app.mount("#root")` 这句要放在创建组件的后面，创建完成之后才可以挂载节点


## 第二章
### 2.1应用和组件的基本概念

``` js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message:'hello world'
            }
        },
        template:"<div>{{message}}</div>"
    });
    const vm = app.mount("#root");
</script>
```
- createApp 表示创建一个 Vue应用，存储在app变量中
- 传入的参数表示这个应用最外层的组件，应该如何展示
- Vue类似于mvvm设计模式
- vm就是绑定数据和视图的对象，即根组件

### 2.2Vue生命周期函数（1）
> 生命周期函数: 在某一个时刻会自动执行的函数

``` js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message:'hello world'
            }
        },
        beforeCreate(){
            console.log("beforeCreate_log");
        },
        created(){
            console.log("created_log");
        },
        beforeMount(){
            console.log("beforeMount_log");
        },
        mounted(){
            console.log("mounted_log")
        },
        beforeUpdate(){
            console.log("beforeUpdate_log")
        },
        updated(){
            console.log("updated_log")
        },
        beforeUnmount(){
            console.log("beforeUnmount_log")
        },
        unmounted(){
            console.log("Unmounted_log")
        },
        template:"<div>{{message}}</div>"
    });
    const vm = app.mount("#root");
</script>
```
- `beforeCreate(){}` 在实例生成之前自动执行的函数
- `create(){}` 在实例生成之后自动执行的函数
- `beforeMount(){}` 在组件内容被渲染到页面之前自动执行的函数
- `mounted(){}`在组件内容被渲染到页面之前自动执行的函数

### 2.3Vue生命周期函数（2）
- `beforeUpdate(){}`当data中的数据发生变化时会执行
- `updated(){}`当data中的数据发生变化之后会执行 
- `app.unmount()` 让Vue停止接管节点
- `beforeUnmount(){}` 当Vue失效时，
- `unMounted(){}`当vue失效之后

### 2.4常用模板语法讲解(1)
``` js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message:'<strong>hello world</strong>',
                disable:true,
                show:true
            }
        },
        template:`
        <div v-html="message"></div>
        <div v-bind:title="message">你好</div>
        <input v-bind:disabled="disable" />
        <div>{{Math.random()}} </div>
        <div v-once>{{massage}} </div>
        <div v-if="show">{{message}} </div>
        `
    });
    const vm = app.mount("#root");
</script>
```
- `v-html` 让节点中编译message中的html标签
- `v-bind:title="message"` 让message作为变量而不是字符串给title属性
- `{{表达式}}` {{}}中可以存放表达式，不可有语句
  - ps：简单理解表达是有值，语句不总是有值
- `v-once` 让指定的节点值渲染一次数据内容，数据内容改变时，不会重新渲染
- `v-if=true/false` 如果为true则节点展示，如果为false则不展示

### 2.5常用模板语法讲解(2)

``` js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message: 'hello world',
                name: 'title',
                event: 'mouseenter'
            }
        },
        methods: {
            handleClick() {
                alert("click");
            }
        },
        template: `
        <div 
            :[name]="message" 
            @[event]="handleClick"
        > 
            {{message}} 
        </div>

        <form action="https://baidu.com" @click=handleClick>
            <button type="submit"> 提交跳转
            </button>
        </form>

        <form action="https://baidu.com" @click.prevent=handleClick>
            <button type="submit"> 提交不跳转
            </button>
        </form>
        `
    });
    const vm = app.mount("#root");
</script>
```
#### 动态属性
- ``<div v-on:click="handleClick" v-bind:title="message"></div>` 可以简写成 `<div @click="handleClick" :title="message"></div>`
- 在data中定义 `name:'title'` 模板中可以写成 ` :[name]="message"`
- `@click.prevent=handleClick` 可以更改click的默认事件，上述代码中让按钮不跳转

### 2.6数据、方法、计算属性和侦听器
> 在methods中最好不使用箭头函数，箭头函数指向外层函数，this指向的是window

``` js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message: 'hello world',
                count:1,
                price:5
            }
        },
        computed:{
            totle(){
                return this.count*this.price;
            }
        },
        methods:{
            handleClick(){
                console.log("click",this.message);
            },
            formatString(string){
                return string.toUpperCase();
            },
            gettotle(){
                return this.count*this.price;
            }
        },
        template: `
        <div 
        @click="handleClick">{{formatString(message)}}
        </div>
        <div 
        @click="handleClick">{{count*price}}
        </div>
        <div
        @click="handleClick"> {{totle}}
        </div>
        <div
        @click="handleClick"> {{gettotle()}}
        </div>
        `
    });
    const vm = app.mount("#root");
</script>
```
- `computed:{}`当计算属性依赖的内容发生改变的时候，才会重新计算
- `methods:{}`只要页面重新渲染，才会重新计算

### 2.7数据、方法、计算属性和侦听器(2)
```js
<script>
    const app = Vue.createApp({
        data() {
            return {
                message: 'hello world',
                count: 1,
                price: 5,
                newTotle:10,
            }
        },
        //监听器
        watch: {
            //做一个类似computed计算属性的效果
            price(current,prev) {
            this.newTotle=current*this.current;
            }
        },
        computed: {
            totle() {
                return this.count * this.price;
            }
        },
    });
    const vm = app.mount("#root");
</script>
```
- `computed`和`methods`都能实现一个功能，建议使用computed，因为有缓存
- `computed`和`watcher`都能实现的功能，建议使用computed因为更加简洁


### 2.8样式绑定语法
```js
<script>
    const app = Vue.createApp({
        data() {
            return { 
                classString: 'red',
                classObject:{red:false,green:true},
                classArray:['red','green',{brown:false}],
                styleString:'color:yellow;',
                styleObject:{
                    color:"orange",
                    background:'yellow'
                }
            }
        },
        template: `
        <div :class="classString">
            hello world 
            <demo class="green"/>
        </div>

        <div :style="styleString">
            我样式绑定的是一个字符串
        </div>
        <div :style="styleObject">
            我样式绑定的是一个对象
        </div>
        
        `
    });
    app.component('demo',{
        template:`
         <div :class="$attrs.class">one</div> 
         <div>two</div> 
         `
    })
    const vm = app.mount("#root");
</script>
```
> 样式绑定有5种，绑定语法可以在上述代码中的`template`中找到

1. `classString:'red'`通过`v-bind`绑定`class`属性来绑定样式
2. `classObject:{red:false,green:true}`通过类的对象来绑定样式，可以控制哪些样式显示，哪些样式不显示。
3. `classArray:['red','green',{brown:false}]`通过绑定数组来绑定样式，数组中可以包含对象。
4. `styleObject:{color:"orange",background:'yellow'}` 绑定数据对象（常用）

### 2.9条件渲染
```js
<script>
    const app = Vue.createApp({
        data() {
            return {
                show: false,
                conditionOne:false,
                conditionTwo:true,
            }
        },
        template: `
        <div v-if="show"> Hello World</div>

        <div v-if="conditionOne"> If</div>
        <div v-else-if="conditionTwo">Else If</div>
        <div v-else="else"> Else</div>

        <div v-show="show"> Bye World</div>
        `
    });
    const vm = app.mount("#root");
</script>

```
1. `v-if="conditionOne"`去除DOM元素达到隐藏节点的效果
2. `v-else="else"` 如果`conditionOne`是true则渲染If，反之则渲染Else
3. `v-else-if="conditionTwo"` 如果`conditionOne`为false，`conditionTwo`为true，则会渲染Else If,两者都为false则会渲染Else
4. `v-show` 通过改变样式`style="display: none;"`来隐藏节点

### 2.10-2.11列表循环渲染（1-2）
```js
<script>
    const app = Vue.createApp({
        data() {
            return {
                listArray: ['dell', 'lee', 'teacher'],
                listObject: {
                    firstName: 'dell',
                    lastName: 'lee',
                    job: 'teacher'
                },
            }
        },
        methods:{
            handleAddBtnClick(){
                // this.listArray.push("hello")
                // this.listArray.pop()
                // this.listArray.shift()
                // this.listArray.unshift('hello')
                // this.listArray.reverse()
                // this.listArray=['bye','world']
                // this.listArray=['bye'].concat(['world'])
                // this.listArray=['bye','world'].filter(item => item==='bye') //滤出元素为bye的元素
                // this.listArray[1]='hello'

                this.listObject.age=100;
                this.listObject.sex="male";
            }
        },
        template: `
        <div>
            <div v-for="(item,index) in listArray" :key="index">
            {{item}}---{{index}}
            </div>
            

            <div
             v-for="(value,key,index) in listObject" 
             :key="index"
             v-if="key !=='lastName'" //不起作用
            >
            {{value}}---{{key}}
            </div>


            <template
             v-for="(value,key,index) in listObject" 
             :key="index"
            >
            
            <div v-if="key !=='lastName'">  
            //把v-if放在单独的div中，来进行判断
                {{value}}---{{key}}
            </div>

            </template>

            <button @click="handleAddBtnClick" >新增</button>
        </div>
        `
    });
    const vm = app.mount("#root");
</script>
```
1. 使用`v-for`渲染列表的时候，一般会在节点上加上`:key="index"`唯一的值，这样在添加、减少数值时Vue就不会重新渲染前面未做更改的节点
2. 更改列表数据：
    1. 使用数组的变更函数
        1. `this.listArray.push('123')`:在数组的末端加上数据
        2. `this.listArray.pop()`：从数组的末端删除数据
        3. `this.listArray.shift()`：从数组的前端删除数据
        4. `this.listArray.unshift('hello')`从数组的前端添加数据
        5. `this.listArray.reverse()`:将数组的元素反向1234->4321
    2. 直接改变数组
        1. `this.listArray=['bye','world']`直接替换掉原有数组
        2. `this.listArray=['bye'].concat(['world'])` 用concat方法连接多个数组
        3. `this.listArray=['bye','world'].filter(item => item==='bye')` 滤除数组中为数值为bye的元素
    3. 直接更改数组的内容
        1. `this.listArray[1]='hello'`改变数组中具体的某个元素
3. 更改对象的内容：`this.listObject.sex=male;`直接添加一个属性
4. 渲染对象的时候，如果不想要渲染某个属性/字段可以用下面的`template`标签类似于一个占位符，在DOM不渲染，因为`v-for`的优先级比`v-if`优先级要大，如果两者在一上一下，`v-if`不起作用，
