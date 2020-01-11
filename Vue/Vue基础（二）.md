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

深入部分
<a name="dmtvD"></a>
### 组件名
当直接在DOM中使用一个组件时（不是字符串模版和单文件组件），强烈推荐使用W3C规范的自定义组件名，即 字母全小写，且必须包含一个连字符，这会帮助你避免和当前和未来的HTML元素冲突。

组件名大小写：<br />使用短横线分隔命名： my-component-name<br />使用首字母大写：MyComponentName<br />直接在DOM中使用组件的时候，只有 短横线分隔命名法 是有效的。

<a name="jP0v3"></a>
### 全局注册

```javascript
Vue.component('my-component-name',{
	// ..options
})
```

在注册之后可以在任何新创建的Vue根实例的模版中使用。

<a name="Eff43"></a>
### 局部注册
可以通过一个普通的JavaScript对象来定义组件。


```javascript
var ComponentA =  {}
```

然后在根实例的components选项中定义你想要使用的组件

```javascript
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

<a name="nHGNM"></a>
### 模块系统
推荐创建一个components目录，并且将每个组件放在各自的文件中。<br />然后你需要在局部注册之前导入每个你想使用的组件。<br />可能你有许多相对通用的组件，称之为基础组件，会在各个组件中被频繁的用到。

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

深入部分<br />任何类型的值都可以传给一个prop。但是除了字符串之外，其余类型的值都需要使用v-bind指令通知Vue。<br />传入一个对象的所有属性：

```javascript
<blog-post v-bind="post"></blog-post>
=> 等价于
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

<a name="FZ8Ue"></a>
### Prop验证
可以为组件的prop指定验证要求。

```javascript
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

<a name="6HV7E"></a>
### 单向数据流
所有的prop都使得其父子prop之间形成了一个单向下行绑定，即数据从父级节点流向子节点。<br />不应该在一个子组件内部改变prop。<br />常见的情况：

1. 这个prop用来传递一个初始值： 这个子组件中接下来希望将其作为一个本地的prop数据来使用。这种情况下最好定义一个本地的data属性并将这个prop用作其初始值。
1. 这个prop以一种原始的值传入 且需要进行切换。在这个情况下，最好使用这个prop的值来定义一个计算属性。
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

```javascript
<input v-model = "searchText">
 => 等价于
 
 <input
	v-bind:value = 'searchText'
	v-on:input = 'searchText = $event.target.value'
>
```

v-model指令是一个语法糖。<br />在组件上使用：

```javascript
<custom-input
	v-bind:value = 'searchText'
	v-on:input = "searchText = $event"
></custom-input>
```

组件内的input必须：

- 将其value特性绑定到一个名叫value的prop上，
- 将其input事件被触发时，将新的值通过自定义的input事件抛出

```javascript
// 组件内应该怎么定义定义
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

现在v-model指令就可以在这个组件上工作起来了。

```javascript
<custom-input v-model="searchText"></custom-input>
```

深入部分

<a name="5KG6M"></a>
## 通过插槽分发内容
为了解决无法向组件内传递一段HTML代码。<br />可以使用v-html，但是这段HTML代码上的数据绑定和事件监听不会生效。

深入部分
<a name="3YXTB"></a>
## 动态组件
在不同组件之间切换。<br />可以通过Vue的component元素加一个特殊的is特性来实现。

```javascript
<component v-bind:is = 'currentTabComponent'></component>
```

currentTabComponent 可以是一个已注册组件的名称 或者一个组件的选项对象。

深入部分

<a name="murSE"></a>
## 解析DOM模版时的注意事项
有些DOM元素比如 ul table等，对于哪些元素可以出现在其内部是有严格的限制的。而有些元素 比如 li，tr等只能出现在特定的元素内部。

```javascript
<table>
  <blog-post-row></blog-post-row>
</table>
```

这个自定义组件blog-post-row 会被作为无效的内容提升到外部，并导致最终渲染效果出错。<br />可以使用is特性来解决这个问题。

```javascript
<table>
  <tr is="blog-post-row"></tr>
</table>
```

需要注意的是，如果模版的来源是以下的情况下，这个限制是不存在的：

1. 字符串
1. 单文件组件
1. <script type="text/x-template"></script>