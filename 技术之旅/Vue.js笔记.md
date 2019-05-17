# Vue.js笔记

1. ## 常用命令

   * 安装webpack

     ```bash
     npm install webpack [-g]		//-g代表global，表示将webpack安装到全局环境中
     ```

   * 安装vue脚手架

     ```shell
     npm install vue -cli [-g]
     ```

   * 项目常用命令

     ```shell
     /** 通过webpack创建vue项目工程 */
     vue init webpack project-name		//project-name为你的工程名，不能用中文，建议用小写

     /** 指定vue的版本创建vue项目工程 */
     vue init webpack#1.0 project-name	//创建vue1.0的项目

     /** 安装项目依赖 */
     npm install 							//安装基本依赖

     /** 安装vue路由模块vue-router和网络请求模块vue-resource */
     npm install vue-router vue-resource

     /** 启动项目 */
     npm run dev
     ```



2. ## 生命周期
   1. created：  created是vm实例已创建但未挂载。
   2. mounted：  一些DOM操作应该放在mounted中。


3. ## 计算属性--cumputed
    * 使用计算属性时，当值发生改变时，便会出发计算属性重新计算，让后将计算结果更新到页面
    * watch属性是当某一参数发生变化便会执行。其作用是弥补，当需要在数据变化时执行异步或开销较大的操作时,computed属性无法高效处理时的不足。


4. ## 组件通信
	1. 父子通信：在Vue.js中，父子组件的关系可以总结为 props down, events up 。父组件通过props向下传递数据给子组件，子组件通过events给父组件发送消息。
		1. 父->子： props，prop是单向绑定的，当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。另外，每次父组件更新时，子组件的所有prop都会更新为最新值。这意味着你不应该在子组件内部改变prop。
		* <font color=#ff0000>注：在JavaScript中对象和数组是引用类型，指向同一个内存空间，如果prop是一个对象或数组，在子组件内部改变它会影响父组件的状态。</font>
		2. 子->父：通过events事件传递消息（`$emit`和`$on`）

	2. 非父子通信：有时候两个组件也需要通信(非父子关系)。在简单的场景下，可以使用一个空的Vue实例作为中央事件总线。在复杂的情况下，我们应该考虑使用专门的状态管理模式。

5. ## 官网VUE教程笔记
	1.  ### 模板语法：
		1.  使用`{{}}`在`<template>`中引用模块中绑定的变量或js数值表达式，添加`v-once`属性后将使该变量不再更新。	
		2.  `v-html="rawHtml"`可以输出带html格式的变量`rawHtml`样式效果。
		3.  标签元素自身属性在其属性名前加`v-bind:`或`:`即可引用变量来作为属性值，当属性对应的是方法名时，使用`v-on:`或`@`。
		4.  修饰符 (Modifiers) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`;  
   			``` html   
			<form v-on:submit.prevent="onSubmit">...</form>
			```
		5. 计算属性 VS 方法： 两者的不同点是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。
		6. 计算属性默认只有`getter`方法，可以在需要时，自己提供一个`setter`方法
			``` javascript
			// ...
			computed: {
  				fullName: {
    				// getter
    				get: function () {
      					return this.firstName + ' ' + this.lastName
    				},
    				// setter
    				set: function (newValue) {
      					var names = newValue.split(' ')
      					this.firstName = names[0]
      					this.lastName = names[names.length - 1]
    				}
  				}
			}
			// ...
			```
			7. **侦听器**：Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。
			8. 可以通过`v-bind`或`:`来给`class`属性和`style`属性进行样式的动态绑定。
				``` html
				<!-- class属性的动态绑定 -->
				<div v-bind:class="[{ active: isActive }, errorClass]"></div>
				
				<!-- style样式动态绑定 -->
				<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
				```

	2. ### 条件渲染 (`v-if` and `v-show`)
		1. `v-if="ok"`中当ok为true时该元素才会显示
		2. `v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。
			```  html
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
			```
		4. Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。通过添加一个具有唯一值的`key`属性即可不进行复用。
			```  html
			<template v-if="loginType === 'username'">
  				<label>Username</label>
  				<input placeholder="Enter your username" key="username-input">
			</template>
			<template v-else>
  				<label>Email</label>
  				<input placeholder="Enter your email address" key="email-input">
			</template>
			```	
		5. `v-show`指令效果和`v-if`差不多，不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS 属性 display。
			* <font color=#ff0000>*注意，`v-show` 不支持 `<template>` 元素，也不支持 `v-else`*</font>
		6. `v-if` VS `v-show`
			* `v-if`在条件为真时才开始渲染，否则什么都不做，`v-show`不管初始条件是什么，元素都会被渲染。
	3. ### 列表渲染 (`v-for`)


6. ## vue 父子组件的生命周期顺序
	1. 加载渲染过程
	父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mounted
	2. 子组件更新过程
	父beforeUpdate -> 子beforeUpdate -> 子updated -> 父updated
	3. 父组件更新过程
	父beforeUpdate -> 父updated
	4. 销毁过程
	父beforeDestroy -> 子beforeDestroy -> 子destroyed -> 父destroyed