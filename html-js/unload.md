## unload (都是泪）

具体问题如下：
#### 1. onunload 事件无法 阻断 任何浏览器关闭页面的操作
代码1：
``` html
	<html>
	<head>
		<title></title>
	</head>
	<body onunload="alert('are you sure???')">
	</body>
	</html>
```
代码2：
``` html
	<html>
	<head>
		<title></title>
	</head>
	<body >
		<script>
			window.onunload=function(){
				alert("are you sure?");
			}
		</script>
	</body>
	</html>
```
2. 查了资料发现要用 onbeforeunload 
3. 用了onbeforeunload之后，发现IE浏览器可以，Chrome 和Firefox还是不行。

2  3 问题 代码如下
代码3：
``` html
	<html>
	<head>
		<title></title>
	</head>
	<body >
		<script>
			window.onbeforeunload=function(){
				return "are you sure?";
			}
		</script>
	</body>
	</html>
```
4. 很神奇的一点是，用onbeforeunload在Chrome和Firefox打断点那能出效果，不打断点还是不会阻止。

 **尼玛反正就是咋试在Chrome和Firefox都不行 (▼へ▼メ)**

----------

百度求助无门后翻墙看国外MDN。
> [MDN unload](https://developer.mozilla.org/en-US/docs/Web/Events/unload)
> [MDN beforeunload](https://developer.mozilla.org/en-US/docs/Web/Events/beforeunload)

自己整理了解了一些，好像找到问题了(ಥ_ಥ) 

----------

### unload

> The unload event is fired when the document or a child resource is being unloaded.

意思就是，unload事件会在  正在卸载文档和子资源的时候触发。

这个事件会在  ```beforeunload``` 和 ```pagehide``` **之后** 触发
（仔细解读上面那句话哦！）

 unload 事件触发时的文档在一种特定的状态：
 - 所有资源仍然存在（img，iframe等）
 - 最终用户看不到任何东西
 - UI交互无效（window.open,alert,confirm等）
		所以代码1和代码2中的alert是无效的，会报错。
 - 报错不会停止卸载该工作流程
	 意思就是即使报错了，这个页面继续卸载。
	 
 **所以尼玛unload这东西到底有啥用呢？？？反正页面最后也得关  ヽ(。_°)ノ**
 
 我自己想了想，可能有下面几种作用
 1. 浏览器强制关闭处理：
	 
	 如下代码：
	``` html
		<!DOCTYPE html>
		<html>
		  <head>
			<title>Child Frame</title>
		  </head>
		  <body>
			  ☻
			<script>
			  window.onunload=function(){
				localStorage.setItem("close","yes");
			  }
			</script>
		  </body>
		</html>
	```
	如果浏览器强制关机的话我们可以有一些操作，比如把一些存到本地以便下次打开该页面的时候再用，或者在onunload里面调接口，进行一些操作等等。

----------

### beforeunload

当窗口、文档还有它的资源即将卸载的时候触发此事件，文档依旧可见，事件仍可取消。

如果为returnValue 这个事件属性赋值一个字符串，则会弹出一个对话框要求用户确认离开该页面。有些浏览器会按照returnValue的值来显示提示框中的内容，也有一些浏览器展示他们自己的内容。如果没有提供returnValue的值，则按照静默方式处理。

**注意**  为了防止不需要的弹窗，浏览器可能不会展示在beforeunload事件中创建的提示信息，除非已经交互过了，否则根本不会展示他们。
**mmp，这会不会是我用beforeunload也不显示弹窗的原因? (；′⌒`)** 

HTML规范声明作者应使用Event.preventDefault（）方法而不是使用Event.returnValue。 但是，所有浏览器尚不支持此功能。
如下代码
``` script
	window.addEventListener("beforeunload", function (event) {
	  // Cancel the event as stated by the standard.
	  event.preventDefault();
	  // Chrome requires returnValue to be set.
	  event.returnValue = '';
	});
```

**HTML规范声明在此事件期间可能会忽略对window.alert（），window.confirm（）和window.prompt（）方法的调用。
各种浏览器忽略事件的结果，并且根本不要求用户进行确认。 文档将始终自动卸载。**

意思就是很多时候这个事件没用。

 