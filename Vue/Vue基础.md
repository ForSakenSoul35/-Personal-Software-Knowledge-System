# Vue基础 (一) Vue实例

<a name="vmy7W"></a>
# Vue实例
每个Vue应用都是通过Vue构造函数创建一个新的Vue实例开始的。<br />一个Vue应用由一个通过new Vue创建的根Vue实例，以及可选的嵌套的，可服用的组件树组成。<br />创建一个Vue实例的时候，可以传入一个选项对象，可以使用这些不同的选项来创建一些行为。<br />基本语法如下：

```javascript
var vm = new Vue({
	el:'#app',
  data:{},
  methods:{},
  created:function(){},
  computed:{},
  watch:{},
})
```

<a name="Z4OlA"></a>
## el
el选项 表示这个Vue实例接管的DOM节点区域。
<a name="6r5sj"></a>
## data
表示这个Vue实例的数据。<br />当一个Vue实例被创建时，将data对象中的所有的属性加入到Vue的响应式系统中。当这些属性在methods或者生命周期函数中被修改时，视图中的数据也会**响应式**的被修改，即视图会进行重新渲染。<br />**注意：**<br />获取一个Vue实例的data属性，在Vue实例中可以直接通过 this.属性名获取，还可以在全局范围中使用Vue实例名.$data.属性名来获取。<br />即Vue向外暴露了一些有用的实例属性和方法，都带有前缀 $ ，以便以用户自定义的属性区分开来。
<a name="EtgqQ"></a>
### 模版语法
数据绑定。让DOM中的数据和Vue实例中的数据能够发生响应。
<a name="AgMXP"></a>
#### 插值表达式
数据绑定最常见形式就是使用双大括号的插值表达式。<br />**插值表达式 一般用于 显示普通文本。**<br />插值表示式不仅仅支持普通文件，也支持一些简单的JavaScript表达式。
```html
<span> Message: {{ msg }}</span>
// msg 为vue实例的数据对象上的 属性。
```

插值表达式 会在页面被渲染时，自动将插值表达式中的值替换成 Vue实例数据对象中属性的值。<br />并且无论何时，绑定的数据对象上属性的值 发生改变，插值处的内容都会自动更新。
<a name="BHzqV"></a>
### 指令
指令是带有v-前缀的特殊特性。指令特性的值的预期是单个JavaScript表达式。<br />指令的职责是，当表达式的值发生改变时，将其连带的影响，响应地作用于DOM。<br />指令一般用于对HTML元素的特性做一些处理。（改变class类名，改变id等等。）<br />一些指令有特殊的含义。比如v-if 用来控制HTML元素的显示和隐藏。<br />一些指令能够接收一个参数，在指令名称之后 以冒号显示。比如v-bind指令。

```html
<a v-bind:href="url"></a>
// 这里href是参数，告知v-bind指令 该元素的href特性和 Vue实例的数据对象上的url属性的值 绑定。
```
 <br />又比如v-on指令，用于监听DOM事件。

```html
<a v-on:click ="doSomething"></a>
// 这里参数是监听的事件名。
```

**动态参数**<br />从2.6.0开始。可以用方括号起来的JavaScript表达式作为一个指令的参数。

```html
<a v-bind:[attributeName]="url"> ... </a>
```

**修饰符**<br />修饰符是以半角句号指明的特殊后缀，用于指出一个指令应该以特殊的方式绑定。
<a name="COble"></a>
#### v-bind 属性绑定
缩写： v-bind:class  => :class<br />v-bind用于绑定HTML属性。<br />最简单的使用：

```html
<div v-bind:title ="titleName"></div>
<script>
	...
  data:{
  	titleName:'hello title'
  },
  ...
</script>
```
 <br />语法格式： v-bind:class ="classProperty"<br />除了接收字符串类型的变量，还支持单一JavaScript表达式。<br />可以进行简单的运算，执行函数等。<br />除了接收字符串的变量，还支持其他的数据类型作为classProperty<br />接收为对象类型时，会自动调用对象的toString（）方法，并将值作为classProperty。<br />接收为数组类型时，会自动执行arr.join()方法。<br />基于上面的情况。Vue对class和style这两个html属性进行了一定程度的增强。<br />当检测到obj的某个属性值为真时，则将这个属性的属性值添加到绑定元素的class上。

对象语法：<br />classProperty接收一个对象。<br />当对象的某个属性值为true时，会将对应的属性名添加到元素对应的class列表中。
```html
<div v-bind:class ="titleName"></div>
// titleName为一个对象
<script>
	...
  data:{
  	titleName:{
    	active:true,
      title:false
    }
  },
  ...
</script>
// 渲染为 
```


数组语法：<br />classProperty可以是一个数组，以应用一个classList列表。

```html
<div v-bind:class ="[ isActive ? activeClass : '', errorClass]"></div>
// 当isActive为true时，class 列表为 activeClass，errorClass
// 当isActive为false时，class 列表为 errorClass
<script>
	...
  data:{
  	
  },
  ...
</script>
// 渲染为 
```


针对style的扩展<br />对象语法

```html
<div v-bind:style="styleObject"></div>
...

data: {
  styleObject: {
    color: 'red',
    fontSize: '12px'
  }
}
...
```

数组语法<br />可以将多个样式对象应用到同一个元素上

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

Vue.js还会自动侦测 并添加对应的前缀

<a name="BGWaF"></a>
#### v-on  事件处理
缩写 @click

监听DOM事件，并在触发时运行一些代码<br />事件修饰符<br />修饰符是由点开头的指令后缀来表示的。

- .stop 阻止事件冒泡
- .prevent 阻止默认事件
- .capture 内部元素触发的事件 先在这里处理，再交由内部元素处理
- .self 即事件不是从内部元素触发的
- .once 点击事件只会触发一次
- .passive 
<a name="eCtMA"></a>
#### v-for 列表渲染
可以使用v-for指令基于一个数组来渲染一个列表<br />v-for指令需要使用 item  in items的形式。<br />在v-for块中，可以访问所有父作用域的属性。<br />也可以使用v-for遍历对象，可以提供第二个参数 键名 name，第三个参数 index 索引。<br />遍历对象时，会按Object.keys（）的结果遍历。

注意：<br />当Vue正在更新v-for渲染的元素列表时，默认使用 就地更新 的策略。 如果数据源的顺序被改变，Vue将不会移动DOM元素来匹配数据项的顺序。而是 就地更新。<br />为了跟踪每个节点的身份，从而重用和重新排序现有的元素，需要为每项提供一个唯一的key值。

<a name="OuuZw"></a>
#### v-if 条件渲染
v-if 
```javascript
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

v-else元素必须紧跟在v-if或者v-else-if后面。<br />使用key来管理 可复用的 元素。 给一个元素添加一个具有唯一值的key属性时，表示这个元素和其他任何同类元素都是不同的，每次切换时都会被重新渲染。

类似指令：v-show<br />根据条件展示元素的选项。<br />不同的是

1. v-show的元素始终会被渲染并被保留在DOM中。v-show只是简单地切换元素的CSS属性display
1. v-show不支持在template标签上使用。v-if支持。
1. v-if是惰性渲染的。只会在条件为真时才会渲染。v-show则不管条件是否为真，第一次的时候都会渲染。
1. v-if有更高的切换开销。v-show有更高的初始渲染开销。 如果需要频繁的切换使用v-show，否则使用v-if。

不推荐v-if和v-for一起使用。<br />一起使用时 v-for具有v-if更高的优先级。

<a name="G3Ywv"></a>
#### v-model 表单输入绑定 绑定value属性？
可以使用v-model指令在表单 input textarea 以及select元素上创建双向绑定。<br />根据控件类型自动选取正确的方法来更新元素。<br />v-model只不过是语法糖。<br />负责监听用户的输入事件，以更新数据，并且对一些极端场景进行一些特殊处理。

v-model 会忽略所有表单元素的value，checked，seleceted特性的初始值，总是将Vue实例的数据作为数据来源，应该在data中设置初始值。

对于需要使用输入法的语言，v-model不会在输入法组合文字过程中得到更新，如果想处理这个过程，请使用input事件。（拼写过程中的变化不会做处理）<br />文本框：<br />

```javascript
<input v-model="message" placeholder ="edit me">
```

单个复选框

```javascript
<input type="checkbox" v-model="checked">
```

多个复选框绑定到同一个数组

```javascript
<input type="checkbox" v-model="checkedName">
<input type="checkbox" v-model="checkedName">
<input type="checkbox" v-model="checkedName">
<input type="checkbox" v-model="checkedName">
```

单个单选框

```javascript
<input type="radio" v-model="picked">
```

多个单选框<br />同一组的单选框绑定到同一个值

```javascript
<input type="radio" v-model="picked">
<input type="radio" v-model="picked">
```

下拉框<br />单选 下拉框
```html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

注意： 如果v-model表达式的初始值未能匹配任何选项，<select>元素将被渲染未选中状态。在iOS中会导致无法选择第一个选项。因此，iOS不会触发change事件，所以推荐提供一个值为空的禁用选项。

多选下拉框<br />通过v-model绑定到同一个数组。<br />

上面描述的都是选中状态的绑定。<br />值的绑定。<br />对于单选按钮，复选框v-model绑定都是静态字符串

```html
<input type= "radio" v-model="picked" value="a">// 当选中时 picked的值为“a”
<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>

```

如果想要绑定的值为一个Vue实例上的动态属性，这是可以使用v-bind实现，并且这个值可以不是字符串。

```html
<input type="radio" v-model="pick" v-bind:value="a">
// 当选中时
//vm.pick === vm.a

```

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

修饰符<br />.lazy 在默认情况下 v-model 在每次input事件触发之后会将输入框的值与数据进行同步。可以添加.lazy修饰符。从而改变使用change事件进行同步。<br />.number 自动将用户的值转为数值类型，可以给v-model添加number修饰符。<br />.trim 自动过滤用户输入的首尾空白字符。

<a name="xo1nJ"></a>
#### v-html
双大括号（插值表达式）会将数据解释为普通文本，而非HTML代码（不会解析HTML）。为了输出真正的HTML，需要使用v-html指令。

```html
<p>Using v-html <span v-html="rawHtml"></span> </p>
// rawHtml 为一段html代码


```

**注意：**

1. 不能使用v-html来复合局部模版（即 这段html代码上还带着其他的数据绑定），直接作为HTML会忽略掉属性值中的数据绑定。
1. 请只对可信内容使用HTML插值，绝不要对用户提供的内容使用插值。很容易导致XSS攻击。
<a name="aqjF4"></a>
## methods
<a name="UiSfW"></a>
## created
created方法是Vue实例自带的方法，属于Vue实例的生命周期方法。除了created之外，还有其他的生命周期方法。<br />每个Vue实例在被创建时都要经过一系列的初始化过程。例如，设置数据监听，编译模版，挂载DOM并在必要的时候更新DOM。在这个过程中，也会自动的执行一些生命周期函数，这些生命周期函数会在Vue生命周期的不同阶段被调用。<br />Vue的生命周期函数如下：创建，挂载，更新，销毁

  - 初始化事件 生命周期
- beforeCreate
  - 初始化注入 和校验
- created
  - 判断是否有el和template选项
- beforeMount
  - 创建vm.$el 替换 el
- mounted
  - 挂载完毕 
  - 当data被修改后，虚拟DOM重新更新之前
- beforeUpdate
  - 虚拟DOM更新之后
- updated
  - 执行vm.$destroy 
  - 解除绑定 销毁子组件之以及事件监听器之前
- beforeDestroy
  - 销毁完毕
- destroyed

**注意：**<br />不要在Vue的数据属性以及生命周期回调函数上使用箭头函数，会导致函数中this指向发生错误。
<a name="1BrTT"></a>
## computed
computed即计算属性。<br />模版语法中表达式十分便利，但是在模版中放入太多的逻辑会让模版过重且难以维护。对于复杂的逻辑，应该使用计算属性。<br />computed是一个对象，对象中的每个属性都是一个函数，属性的值由函数的返回值决定。

```javascript
var vm = new Vue({
	el:'#app',
  data: {
  	message:'Hello'
  },
  computed:{
   // 定义了 reverseMessage 计算属性，可以在对应的html中 使用插值表达式 {{reverseMessage}}
  	reverseMessage: function() {
    	// this 指向vm实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

我们这里声明了一个计算属性 reverseMessage， 提供的函数将用作属性 vm.reverseMessage  的 getter函数。

**Vue知道vm.reverseMessage依赖于message，当message发生变化时，所有依赖vm.reverseMessage的绑定也会更新。**

<a name="Zeu6u"></a>
### 计算属性和方法
我们也可以将vm.reverseMessage 对应的函数写在methods方法中，然后在插值表达式调用这个方法，也能得到对应的值。<br />不同的是，计算属性是基于它们的响应依赖进行缓存的。只在相关响应式依赖发生变化时它们才会重新求值，而方法每次都会执行。<br />如果不希望有缓存的效果，请使用方法代替。
<a name="RlTBm"></a>
## watch
Vue提供了一种更通用的方法来观察和响应Vue实例上的数据改动: 侦听属性，也就是watch。<br />当你有一些数据需要随着其他数据变动，很容易滥用watch。<br />通常更好的做法是使用计算属性而不是命令式watch的回调。

```javascript
var vm = new Vue({
	el:'#app',
  data: {
  	firstName: 'Foo',
    lastName: 'Bar',
    fullName: ' Foo Bar'
  },
  watch: {
  	firstName: function(val){
    	this.fullName = val + '' + this.lastName
    },
    lastName: function(val) {
    	this.fullName = this.firstName+ '' + val
    }
  }
})
```

计算属性默认只有getter。不过在需要时也可以提供一个setter。

```javascript
...
computed: {
	fullName: {
  	// getter
    get: function(){
    	return this.firstName + '' + this.lastName
    },
    // setter
    set: function(newValue){
    	var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[ names.length - 1]
    }
  }
}
```

虽然计算属性在大多数情况下比较合适，但有时候也需要一个自定义的侦听器。这就是为什么Vue通过watch选项提供一个更通用的方法，来响应数据的变化。<br />**当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。**

使用watch选项 允许我们执行异步操作，限制我们执行该操作的频率，并且在我们得到最终结果之前，设置中间状态，这些都是计算属性无法做到的。
