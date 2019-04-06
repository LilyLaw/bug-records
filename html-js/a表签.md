## a标签踩坑

**a标签是行内元素，不是块级元素**

使用a标签 href 实现锚点跳转，效果与预期不符，主要原因如下

1. 页面内容高度没有占满，无法完成跳转，如下代码

	``` html
	<section>
		<a href="#jumpdes">点我跳转</a>
		<div>
			<img src="../img/15.jpg" alt="" style="width: 400px;height: auto;">
		</div>
		<p id="jumpdes">我是跳转的目的地</p>
	</section>
	```

2. 使用JavaScript实现跳转，但胡乱使用a标签，导致页面刷新看不到跳转效果

	正确做法是，(1) 不用a标签；(2) a标签href属性值为 javasript:; 具体写法 href="javascript:;" 如下代码：

	``` html
	<section>
		<a href="javascript:;" onclick="toJumpDes()">点击我跳转到目的地</a>
		<img src="../img/15.jpg" alt="">
		<img src="../img/15.jpg" alt="">
		<p id="destination">我是目的地</p>
		<img src="../img/15.jpg" alt="">
		<img src="../img/15.jpg" alt="">
	</section>
	<script type="text/javascript">
		function toJumpDes(){
			document.getElementById("destination").scrollIntoView();
			console.log('hhhhh'); //注意看控制台，可看到刷新效果
		}
	</script>
	```