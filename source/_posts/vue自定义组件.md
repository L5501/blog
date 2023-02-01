---
title: vue自定义组件
description: vue自定义组件
data: 2022-12-31
updata: 2022-12-31 
tags: 
  - vue笔记
categories:
  - vue
---
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
		<title>组件</title>
	</head>
	<body>
		<div id="app">
			<!-- 1.自定义标签 -->
			<mentaltest></mentaltest>
			<!-- 2.自定义标签绑定固定值 -->
			<jumparading num="二"></jumparading>
			<!-- 3.自定义标签绑定变量 -->
			<hunter v-bind:three="number"></hunter>
		</div>
	</body>
	<script>
		// 1.自定义标签
		Vue.component('mentaltest',{
			template: '<h1> 这是第一个组件 <h1>'
		})
		// 2.自定义标签绑定固定值
		Vue.component('jumparading',{
			props: ['num'],
			template: '<h2> 这是第{{num}}个组件 <h2>'
		})
		// 3.自定义标签绑定变量
		Vue.component('hunter',{
			props: ['three'],
			template: '<h2> 这是第{{three}}个组件 <h2>'
		})
		var app = new Vue({
			el: '#app',
			data:{
				number:'三'
			},
		})
	</script>
</html>
```