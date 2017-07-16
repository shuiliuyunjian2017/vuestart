# Vue基础 #
1. 创建一个**Vue实例**

		<div id="example">显示第一个值:{{message}}</div>
	
		var vm = new Vue({
			el: '#example',
			data: {
				message: 'Hello~' 
			}
		});

2. **v-once**指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定: 

		<div v-once>显示第一个值:{{message}}</div>


3. **v-html**指令生成纯HTML，如下会把此div里的内容替换成rawHtml变量的值。

		<div v-html="rawHtml">来个纯html试试</div>

4. **v-bind**指令绑定属性，如v-bind:id="mId"绑定属性名为id值为mId值的东东。

5. **修饰符**是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
	
		<form v-on:submit.prevent="onSubmit"></form>

6. **计算属性**
	
		<div id="example">
			<p>Original message: "{{ message }}"</p>	
			<p>Computed reversed message: "{{ reversedMessage }}"</p>
		</div>
	
		var vm = new Vue({
		  el: '#example',
		  data: {
		    message: 'Hello'
		  },
		  computed: {
		    // a computed getter
		    reversedMessage: function () {
		      // `this` points to the vm instance
		      return this.message.split('').reverse().join('')
		    }
		  }
		});

    6.1 VS method 计算属性是基于它们的依赖进行缓存的

    6.2 VS watch  computed属性优于命令式的watch回调


7. **class绑定** v-bind**:class **

	7.1 对象语法、
        
        1. <div v-bind:class="{ active: isActive }"></div>
		如果isActive为true，则渲染为<div class="active"></div>


		2. <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
		如果isActive,hasError为真，则class="active text-danger".

		3. <div v-bind:class="classObject"></div>
		绑定一个对象classObject = {active: true, 'text-danger': false};则class的值取决于对象对应属性的属性值。此处为active.
		同时，classObject也可以为一个（同样返回对象的）computed属性值。
	
	7.2 数组语法
	
		1. <div v-bind:class="[activeClass, errorClass]">
    		data: {
    		  activeClass: 'active',
    		  errorClass: 'text-danger'
    		};
		则渲染为：class="active text-danger"(相当于ES6的解构语法？）

		2. 数组里同样可以用表达式，如：
			`<div v-bind:class="[isActive ? activeClass : '', errorClass]">`

		3. 或对象，如：
			`<div v-bind:class="[{ active: isActive }, errorClass]">`

	7.3 用在组件上

		定制的组件上用class的时候，这些类将被添加到根元素上面，这个元素上已经存在的类不会被覆盖。如：
		Vue.component('my-component', {
		  template: '<p class="foo bar">Hi</p>'
		});
		然后在使用它的时候添加一些 class：
		<my-component class="baz boo"></my-component>
		最终被渲染成：
		<p class="foo bar baz boo">Hi</p>
		

8. **绑定内联样式**　v-bind**:style**

	类似于class的绑定。需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀。

	**另：**从 2.3 开始你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值。
		`<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }">`　这会渲染数组中最后一个被浏览器支持的值。在这个例子中，如果浏览器支持不带浏览器前缀的 flexbox，那么渲染结果会是 display: flex。


9. 	条件渲染

	9.1 **v-if** & **v-else-if** & **v-else**

			<div v-if="type === 'A'">
			  A
			</div>
			<div v-else-if="type === 'B'">
			  B
			</div>
			<div v-else-if="type === 'C'">
			  C
			</div>
			<div v-else>
			  Not A/B/C
			</div>

		
	  1.v-if可以单独使用，也可以配合v-else使用，或配合v-else-if&&v-else使用。

	  2.还可以用于template中，如：

			<template v-if="ok">
			  <h1>Title</h1>
			  <p>Paragraph 1</p>
			  <p>Paragraph 2</p>
			</template>

	    最终的渲染结果不会包含 `<template>` 元素。

	  3.用Key值管理复用的元素。

			<template v-if="loginType === 'username'">
			  <label>Username</label>
			  <input placeholder="Enter your username" key="username-input">
			</template>
			<template v-else>
			  <label>Email</label>
			  <input placeholder="Enter your email address" key="email-input">
			</template>

		若不添加key属性，则每次渲染都会复用相同的元素。添加了key，每次切换输入框都将被重新渲染。如上图。

	
	9.2 **v-show** 与v-if大致相同，只是带有 v-show 的元素始终会被渲染并保留在 DOM 中，且v-show 不支持 <template> 语法。v-show 是简单地切换元素的 CSS 属性 display 。

10.  列表渲染

	10.1 **v-for**

	 1.基本用法。	

			<ul id="example-2">
			  <li v-for="(item, index) in items">
			    {{ parentMessage }} - {{ index }} - {{ item.message }}
			  </li>
			</ul>
			var example2 = new Vue({
			  el: '#example-2',
			  data: {
			    parentMessage: 'Parent',
			    items: [
			      { message: 'Foo' },
			      { message: 'Bar' }
			    ]
			  }
			});
			结果：
			Parent - 0 - Foo
			Parent - 1 - Bar

	　　在 v-for 块中，我们拥有对父作用域属性的完全访问权限。 
		
	　　v-for 还支持一个可选的第二个参数为当前项的索引。还可以用 of 替代 in 作为分隔符。如：`<div v-for="item of items"></div>`
	 

	 2.用带有 v-for 的 <template> 标签来渲染多个元素块

	 3.v-for 通过一个对象的属性来迭代。
			
			<div v-for="(value, key, index) in object">
			  {{ index }}. {{ key }} : {{ value }}
			</div>

　　  此处，在遍历对象时，是按 Object.keys() 的结果遍历，无法保证顺序一致。
