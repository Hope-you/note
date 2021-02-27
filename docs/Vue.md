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
