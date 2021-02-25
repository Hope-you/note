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