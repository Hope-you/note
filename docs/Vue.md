## 第一章
### 1.2初学编写hello world
``` vue
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
``` vue
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
```vue
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
``` vue
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

``` vue
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

> ``` vue
> app.component('组件名称',{
>      props:['组件属性1'，'组件属性2'],
>      template:'组件内容'
>      });
> ```

> `app.mount("#root")` 这句要放在创建组件的后面，创建完成之后才可以挂载节点


## 第二章
### 2.1应用和组件的基本概念

``` vue
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

``` vue
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
``` vue
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

``` vue
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

``` vue
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
```vue
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
```vue
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
```vue
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
```vue
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

### 2-12 2-13 事件绑定（1-2）
```vue
<script>
    const app = Vue.createApp({
        data() {
            return {
                counter: 0
            }
        },
        methods: {
            
            handleDivClick(){
                alert("div clicked")
            },
            handleBtnClick() {
                this.counter+=1;
            },
            handleKeyDown(){
                console.log("key down")
            }
        },
        template: `
        <div>
            {{counter}}
            <div @click="handleDivClick">
                <button @click.stop="handleBtnClick">button</button>
            </div>
        </div>


        <div>
            <div @click.self="handleDivClick">
                {{counter}}
                <button @click="handleBtnClick">button</button>
            </div>
        </div>

        <div>
            <input @keydown.enter="handleKeyDown" />
        </div>

        <div>
            <input @click.right="handleKeyDown" />
        </div>
        `
    });
    const vm = app.mount("#root");
</script>
```
1. 事件修饰符
   1. `<button @click="handleBtnClick(1,$event),handleBtnClick1()">button</button>`需要执行多个事件的时候，需要用都好隔开，斌且加上小括号
   2. 点击事件触发的时候会向函数传递一个`event`事件对象。标签中需要写`$event`
   3. `<button @click.stop=`如果某个节点被另外一个节点包含了，并且两个都有相同的点击事件，点击里面的节点时，同时会触发外面节点的点击事件，在里面的节点的`@click`加上`.stop`
   4. `<button @click.self=`如果点击子元素触发了父元素的事件，可以在父元素的事件加上`.self`Vue会判断是不是子元素的事件，如果是子元素的事件就不会触发父元素的事件。
   5. 其他修饰符：`once`只执行一次、`prevent`默认行为、`capture`、`passive`
2. 按键修饰符`enter`、`tab`、`delte`、`backspace`、`up`、`down`、`left`、`right`
3. 鼠标修饰符：`left`、`right`、`middle`
4. 精确修饰符：`exact`  `@click.ctrl.exact="handle()"`按住ctrl键再点击才可以触发，如果按住了其他键是不能触发的，去掉exact就可以触发。
### 2-14 2-15 双向绑定(1-2)
```vue
<script>
    const app = Vue.createApp({
        data(){
            return{
                message:[],
                messageStr:"hello",
                options:[{
                    text:"A",value:"A",
                },{
                    
                    text:"B",value:"B",
                    
                },{
                    text:"C",value:"C",
                }
            ]
            }
        },
        template: `
        <div>
            {{message}}---{{messageStr}}
           <input v-model="message" />
           <br>
           <br>
           <textarea v-model="message" />
           <br>
           <br>
           jack <input type="checkbox"  v-model="message" value="jack"/>
           dell <input type="checkbox"  v-model="message" value="dell"/>
           lee  <input type="checkbox"  v-model="message" value="lee"/>
           <br>
           <br>
           dell <input type="radio"  v-model="messageStr" value="dell"/>
           jack <input type="radio"  v-model="messageStr" value="jack"/>
           lee  <input type="radio"  v-model="messageStr" value="lee"/>
           <br>
           <br>
           <select v-model="messageStr">
            <option v-for="item in options" :value="item.value">{{item.text}}</option>
           </select>
           <br>
           <br>
           <input type="checkbox" 
            v-model="messageStr" 
            true-value="hello"
            false-value="world"
           />
           <br>
           <br>
           <input v-model.lazy="messageStr" />
        </div>

        `
    });
    const vm = app.mount("#root");
</script>
```
1. `v-model`可以绑定表单上的数据，比如`input`、`textarea`、`input type="checkbox"`如果要实现多个复选框，可以绑定一个数组，而且checkbox需要给以一个value值，也就是显示在数组中的那个值。如果要是绑定radio单选按钮，一般不用绑定数组，而是绑定字符串![双向绑定](https://gitee.com/Hope-you/scratch_demo/raw/master/Pic/20210303014746.gif)

2. ```vue
   <select v-model="messageStr">
               <option v-for="item in options" :value="item.value">{{item.text}}</option>
              </select>
   ```

下拉框一般用for循环来渲染

3. 复选框还有一个用法，设置当选中时的value值和未选中的value值。`ture-value="" false-value=""`
4. 修饰符
   - `v-model.lazy` 当输入框失去焦点的时候再更新数据
   - `v-model.number`会自动改变数据的类型
   - `v-model.trim`清除输入框前端和后端的空格，中间的空格不会去除

## 第三章 探索组件的理念
### 3-1 组件的定义及复用性，局部组件和全局组件（1）
``` vue
```
1. 组件具备复用性
2. 组件之间的数据互不影响
3. `app.component('组件名'，{})`定义的是全局组件，随时都可以使用，但是占用性能。  命名规则，小写字母单词，中间用横线间隔
4. 局部组件：定义之后需要在父组件中注册才能使用，性能比较高，使用比较麻烦。命名规则，建议大写字母开头，驼峰命名。
``` vue
    components: {
            Counter,
            'hello-world':HelloWorld,
        },
```
局部组件使用时要做一个名字和组件间的映射对象，不写银蛇，Vue底层也会自动尝试做映射。
### 3-3 组件之间传值校验
``` vue
<script>
    const app = Vue.createApp({
        data(){
            return {
                num:123,
                add: (a,b)=>{
                    alert(a+b)
                }
            }
        },
        template: `
        <div>
            <test content="hello world" />
            <test :content="num" />
            <test1 :add1="add" />
            
        </div>
        `
    });
    app.component('test',{
        props:['content'],//接收数据
        template:`
        <div>
            {{content}}---{{typeof content}}
        </div>
        `
    }),
    app.component('test1',{
        props:{
            add1:Function,//数据校验
        },
        methods:{
            handleClick(){
                this.add1(4,6)
            }
        },
        template:`
        <div @click="this.handleClick">
            {{add1(7,8)}}---{{typeof add1}}
        </div>
        `
    })

    app.component('test2',{
        props:{
            content:{
                type:Number,//校验数据类型
                validator:function(value){
                    return value<1000;
                },//校验传递的数值如果返回true则不会有提示，如果返回false会console警告
                required:true,//校验数据是否为必要的
                default:555//如果父组件没有传数值过来，默认值是什么，默认值可以是数值也可以是函数
            }//额外的数据校验
        },
        methods:{
            handleClick(){
                alert(content)
            }
        },
        template:`
        <div @click="this.handleClick">
            {{content}}---{{typeof content}}
        </div>
        `
    })
    const vm = app.mount("#root");
</script>
```
1. 父组件在创建子组件标签的时候通过属性传参给子组件，子组件中定义`props`属性来接收传过来的数值，
2. 静态传参：属性前面不加`:`只能传字符串
3. 动态传参：父组件中在传参的时候加上`:`可以传数字等。(`:`也就是`v-bind`)
4. 数据校验：`props`用`{content:String}`来接收和校验数据，如果接收的数据不是`String`类型，console可以看到警告
   - 可以校验 String Boolean Array Object Function Symbol(占位符)
   - 数据有很多校验，看代码注释
5. 传输函数：父组件可以传递函数给子组件执行

### 3-4 单项数据流的理解
``` vue
<script>
    const app = Vue.createApp({
        data() {
            return {
                params: {
                    a: 12
                }
            }
        },
        template: `
        <div>
            <test :content="params" />
            <test1 :counter="params.a"/>
        </div>
        `
    });
    app.component('test', {
        props: ['content'],//接收数据
        template: `
        <div>
            <div v-for="(item,key) in content">{{item}}---{{key}}</div>
            {{content}}
        </div>
        `
    })

    app.component('test1', {
        props: ['counter'],//接收数据
        data() {
            return {
                myCounter: this.counter
            }
        },
        template: `
        <div @click="myCounter+=1">
            原始数据{{counter}}<br> 子组件数据{{myCounter}}
        </div>
        `
    })
    const vm = app.mount("#root");
</script>
```
1. 如果传递的数据条目比较多，那么就可以把这些数据封装成对象，然后直接传递给子组件就可以了。
2. 父组件在传递参数的时候`<div :content-abc="变量名"></div>`子组件在`props`中接收的时候可以写成`props:[contentAbc]`,不可以写成`props:[content-abc]`这样是接收不到的。注意传递和接收的格式。
3. 父组件传递给子组件数据的时候，子组件只能使用父组件的数据，不能修改父组件数据内容的。(单项数据流)，可以在子组件中定义data，先把传递过来的数值赋值到一个新的变量中，渲染的时候使用新的变量就可以实现间接修改数据了

### 3-5 Non-props属性
```vue

```
1. Non-prop属性：当父组件给子组件传值的时候，如果子组件没有用`props`接收父组件的数据，