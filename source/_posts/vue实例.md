---
title: vue实例
description: vue实例
data: 2023-1-1
updata: 2021-1-1 
tags: 
  - vue笔记
categories:
  - vue
---
vue实例就是你new出来的vue对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
	</head>
	<body>
		<div id="app">
			<div>{{a}}</div>
		</div>
	</body>
	<script>
		var data = {a:1}
		// 如果在此处使用Object.freeze()方法，则改变data中的值，不会影响到视图。如下
		// Object.freeze(data);
		// 此方法将data变为只读
		
		var app = new Vue({
			el: '#app',
			data: data
		})
		
		// 此处展示data中的a,且此时,data中的a和app中的a是同一个
		console.log(data.a);
		console.log(data.a == app.a);
		
		// 如果修改app中的a,data中的a也会改变，为双向绑定
		app.a = 100;
		console.log(data.a)
		
		// 但是，我在此处，于data实例中加入b元素,app中不会加入b
		data.b = 100
		console.log(app.b)   // undefined
		console.log(data.b)
		// 假如我想一开始就加入一个b元素，但是我不知道b的值，可以定义b为null，例如：var data = {a:1, b:null}
		
		// 获取到el这个元素
		console.log(app.$el==document.getElementById("app"));
		// 获取data
		console.log(app.$data == data);
	</script>
</html>
```

### Vue实例生命周期：

#### 生命周期过程：

1. new Vue()实例化一个vue实例，然后init初始化event和lifecycle
2. 执行beforeCeate生命周期函数
3. beforeCreate执行完后，进行数据初始化，定义data数据，方法以及事件
4. 执行created生命周期函数。执行该函数的时候，可以拿到data下的数据以及methods下的方法，即，可调用方法进行数据请求
5. created执行完后，会判断当前是否有el参数。如果有，会再判断是否有template参数；如果没有，则等待调用$mount(el)方法
6. 确保有了el之后，如果再往下判断是否有template参数。如果有，将template模板转化成render函数；如果没有，则将获取到的el编译成template,然后将这个template转换为render函数
7. 再调用beforeMount
8. 之后产生一个虚拟dom，进行保存，再将rander渲染成真实的dom
9. 调用mounted
10. 然后只有当数据变化时，触发beforeUpdate，将变化后的数据渲染到页面上，并且deforeUpate调用后，会重新生成一个新的虚拟dom，然后和原来的比较，算出最小更新范围，从而更新render之中的数据，再将render渲染成真实dom
11. beforeUpdate之后会执行update，具体过程和上一步差不多
12. 之后就是beforeDestroy，此时仍可进行实例操作
13. 销毁完成后，再执行destroyed

#### 生命周期函数以及应用场景：

- beforeCreate（**创建前**）：常用于加loading
- created（**创建后**）：loading结束后，做一些初始化，实现函数自执行
- beforeMount（**载入前**）
- mounted（**载入后**）：发起后端请求，拿回数据，配合路由钩子做一些事情
- beforeUpdate（**更新前**）
- updated（**更新后**）
- beforeDestroy（**销毁前**）： 你确认删除XX吗？
- destroyed（**销毁后**）：当前组件已被删除，清空相关内容