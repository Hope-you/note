## 1.2初学编写hello world
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
## 1.3编写字符串反转和内容显示隐藏

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
## 1-4 编写TodoList 小功能，了解循环和双向绑定
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
            <button v-on:click="handleAddItem">增加</button>
        <ul>
            <li v-for="item of list">{{item}} test</li>
            <li v-for="(item,index) of list">{{item}}  {{index}} test</li>
        </ul>
        </div>
        `
        
    }).mount('#root');
</script>
```
> `v-for= "item of list" ` 将list中的元素循环到item中

>`v-for="(item,index) of list "` 将索引和元素循环到item中

>`v-model="inputValue"` 和`<input/>`控件双向绑定，变量`inputValue`改变，控件中的数值也会发生改变。

>`this.list.push(this.inputValue);`向`list`数组添加元素