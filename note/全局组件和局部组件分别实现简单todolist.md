实现目标是
![todolist](https://typora-an.oss-cn-hangzhou.aliyuncs.com/img/todolist.gif)

分两步:

1. input框的内容在点击button后,依次到下面的无序列表中.
2. input框在每次提交完后自动清空.

input框通过v-model给Vue实例提供数据
button的点击事件handleBtnClick用来处理提交todo项到无序列表.  |
组件通过替换ul中的li来实现的,组件名是TodoItem.到视图层就是`<todo-item v-bind:content="item" v-for="item in list">  </todo-item>`其实就相当于template中的`<li>{{content}}</li>`
组件通过`props:['content']`来从父组件接受被遍历的list中的item的数据传入到li标签的`{{content}}`中
v-bind往子组件中传值,list的每一项都是item,把item传给了todo-item

#### HTML部分(相同的)

```html
<div id="app">
    <div>
        <input type="text" v-model="todoValue">
        <button @click="handleBtnClick">提交</button>
    </div>
    <ul>
        <!-- 被组件替换掉了 -->
        <!-- <li v-for="item in list"></li> -->

        <!-- 这里命名是把下面注册的组件驼峰的地方变成- -->
        <!-- v-bind在这里是要往子组件传入值 list的每一项值赋给了item,再把item通过v-bind传递给todo-item-->
        <todo-item v-bind:content="item"  
                   v-for="item in list">
            <!-- list变页面也就新增了 -->
        </todo-item>
    </ul>
</div>
```

#### Script部分

1.全局组件写法

```javascript
<script>
    //创建全局组件的方法 组件名叫TodoItem
    Vue.component("TodoItem",{
    //props代表从父组件接受内容(list) 接受一个content的数据
    props:['content'],
    //组件里的模板 接受到的内容 用插值表达式
    template:"<li>{{content}}</li>"
})
var app = new Vue({
    el:"#app",
    data:{
        todoValue:'',
        list:[]

    },
    methods:{
        handleBtnClick:function(){
            this.list.push(this.todoValue);
            this.todoValue=""
        }
    }
})
</script>
```

2.局部组件写法(<font color='red'>局部组件需要在Vue实例中注册</font>)

```javascript
<script>
var TodoItem = {
    props:['content'],
    template:"<li>{{content}}</li>"
}

var app = new Vue({
    // 局部组件使用时需要注册
    components:{
        TodoItem:TodoItem
    },
    el:"#app",
    data:{
        todoValue:'',
        list:[]

    },
    methods:{
        handleBtnClick:function(){
            this.list.push(this.todoValue);
            this.todoValue=""
        }
    }
})
</script>
```

