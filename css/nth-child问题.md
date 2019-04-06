## nth-child 踩坑

一般nth-child 用法如下：
``` html
<style>
	ul>li:nth-child(2n){
		 background: lightgreen;
	}
</style>
<ul>
	<li>item1</li>
	<li>item2</li>
	<li>item3</li>
	<li>item4</li>
	<li>item5</li>
	<li>item6</li>
	<li>item7</li>
	<li>item8</li>
	<li>item9</li>
</ul>
```

但是最近开发过程中遇到了下面的写法，最终效果怎么都出不来：
``` html
<style>
    .wedinfo .infofont:first-child .title{
        animation: tobig 2s linear 1s 1 normal both running;
    }
    .wedinfo .infofont:nth-child(2) .title{
        animation: tobig 2s linear 1.5s 1 normal both running;
    }
    .wedinfo .infofont:last-child .title{
        animation: tobig 2s linear 2s 1 normal both running;
    }
</style>
<div class="wedinfo">
	<div class="headline"><img src="./img/font8.png" alt="invitation">			</div>
	<div class="infofont">
		<div class="title">婚宴</div>
		<p>沉浸在幸福中的我们<br/>敬邀您光临我们的 新婚喜宴</p>
	</div>
	<div class="infofont">
		<div class="title">时间</div>
		<p>2020年10月10日<br/>星期六</p>
	</div>
	<div class="infofont">
		<div class="title">地址</div>
		<p>华天婚礼大酒店<br/>普罗旺斯宴会厅</p>
	</div>
</div>
```

最后注意到infofont的父元素还有一个headline，于是修改css如下正常：
``` stylus
.page2 .wedinfo .infofont:nth-child(2) .title{
    animation: tobig 2s linear 1s 1 normal both running;
}
.page2 .wedinfo .infofont:nth-child(3) .title{
    animation: tobig 2s linear 1.5s 1 normal both running;
}
.page2 .wedinfo .infofont:last-child .title{
    animation: tobig 2s linear 2s 1 normal both running;
}
```

那么nth-child是不是有bug？？？

不是，查阅[官方文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)：
> :nth-child(an+b) 这个 CSS 伪类首先找到所有当前元素的兄弟元素，然后按照位置先后顺序从1开始排序，选择的结果为第（an+b）个元素的集合（n=0，1，2，3...）。

所以要找到所有兄弟元素，而不是同类型的兄弟元素。