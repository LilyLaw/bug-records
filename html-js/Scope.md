## 作用域

先看代码：
```html
<script>
	function myname(){
		let a = b = "sssss";
	}

	myname();

	console.log(b);
	console.log(a);
</script>
```

发现 变量```b```居然可以打印出来？！

a变量报错可以理解，不在作用域范围内。

至于b变量：赋值操作是从右向左的，由于b变量没有用var 来声明，所以b就变成了全局变量。

### 关于全局变量和局部变量

**没有使用var声明的变量，在方法内部或外部都是全局变量**，但如果是在方法内部声明，在方法外部使用之前需要先调用方法，告知系统声明了全局变量后方可在方法外部使用。

```html
<script>
	c = 1;

	function myname(){
		let a = b = "sssss";
	}

	myname();

	console.log(c);		// 1
	console.log(b);		// sssss
	console.log(a);		// 报错
</script>
```

