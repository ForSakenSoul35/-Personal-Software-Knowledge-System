# Vue基础 (二) Vue组件

<a name="Mbg1d"></a>
# Vue组件
组件是可复用的Vue实例，且带有一个名字。<br />可以在一个通过new Vue创建的Vue根实例中，将这个组件作为自定义元素来使用。

组件接收 与Vue实例相同的选项。例如 data，methods，computed，watch等，以及生命周期函数钩子。

注意：<br />data必须是一个函数，返回一个对象<br />为什么？ 因为这样的话，每个Vue实例都可以维护一个被返回对象的独立的拷贝。
<a name="kkLjN"></a>
## 单个根元素
组件的模版template中，只允许有一个根元素。<br />每个组件都必须只有一个根元素。可以将模版的内容包裹在一个父元素中。

<a name="QObe0"></a>
## 组件注册
全局组件和局部组件<br />全局组件注册语法：

```javascript
Vue.component('my-component',{ 
	// ...options
})
```

全局组件可以用在其被注册之后的任何 根实例，也包括其组件树中的所有子组件的模版中（即这个Vue组件的子组件）。


<a name="fAYtQ"></a>
## Prop
通过Prop向子组件传递数据。<br />Prop是在组件上注册的一些自定义特性。当一个值传递给一个prop特性时，就变成了那个组件实例的一个属性。

```javascript
// 组件注册
Vue.component('my-component',{
	props: ['title'],
  template:'<h3>{{title}}</h3>'
})
// 使用
<my-component title="myTitle"></my-component>

```

一个组件可以有任意数量的Prop，任何值都可以传递给任何prop。<br />在组件的模版中，可以直接访问这个值，和组件上的data属性一样。

上面的例子，title是一个静态的字符串。<br />还可以使用v-bind指令，给title传递一个动态的prop。

```javascript
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```


<a name="tkgMP"></a>
## 
<a name="MMjWv"></a>
## 监听子组件事件
子组件需要和父组件进行沟通。<br />Vue实例提供了一个自定义事件的系统来解决父子组件通信的问题。<br />父级组件可以像处理native DOM事件一样通过 v-on监听子组件实例的任意事件。<br />子组件可以通过内建的$emit方法传入事件名称来触发一个事件。

```javascript
<blog-post
  ...
  // 父组件监听
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

```javascript
// 子组件的模版内 触发
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

有时候 子组件还需要向父组件中传递一个具体的值，现在可以使用 $emit（）的第二个参数用来传递，而父组件可以通过$event访问到子组件抛出的这个值。

```javascript
// 子组件内 抛出
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>

```

```javascript
// 父组件 中 使用$event 接收（有疑问？）
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

 或者如果 这个事件处理函数是一个方法时，那么这个值会作为第一个参数传入这个方法

```javascript
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
// 子组件向外派发 enlarge-text 事件， 父组件会监听到。 并且执行onEnlargeText这个方法。
// 子组件向外抛出的参数 会作为这个参数的第一个参数。
></blog-post>
```

```javascript
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

<a name="bCl0g"></a>
## 组件上使用v-model
自定义事件也可以用于创建支持v-model的自定义输入组件。