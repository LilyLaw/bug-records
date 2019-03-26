## GET 提交数据踩坑记录

今天做项目,发现参数遗漏,怎么都传不过去,先看代码:
``` html
<form action="index.php?route=marketing/contact/checksend&user_token=kfkJskdkwejj" method="get">
	<input type="text" placeholder="请输入查询日期,格式如下:2018-01-03" style="width:240px;" name="search_date">
	<input type="submit" value="查询">

	<table>
		<thead>
			<th>id</th>
			<th>邮箱</th>
			<th>状态</th>
			<th>发送时间</th>
		</thead>
		<tbody>
			{% for eli in mailboxs %}
				<tr>
					<td>{{eli.id}}</td>
					<td>{{eli.mailbox}}</td>
					<td>{{eli.status == 1?"已处理":"未处理"}}</td>
					<td>{{eli.date_added}}</td>
				</tr>
	    	{% endfor %}
		</tbody>
	</table>
</form>
```

**注意:** 仔细看form表单的```action```

form表单的action里设置的提交路径里面给url设置了传递参数,而表单方式提交,会不会把url里的参数忽略掉呢???是不是因为这个原因所以route和user_token无法传到controller.

先假设上述想法是对的.并在此基础上修改代码.
``` html
<form action="index.php">
	<input type="hidden" name="route" value="marketing/contact/checksend">
	<input type="hidden" name="user_token" value="{{ user_token }}">
	<input type="text" placeholder="请输入查询日期,格式如下:2018-01-03" style="width:240px;" name="search_date">
	<input type="submit" value="查询">

	<table>
		<thead>
			<th>id</th>
			<th>邮箱</th>
			<th>状态</th>
			<th>发送时间</th>
		</thead>
		<tbody>
			{% for eli in mailboxs %}
				<tr>
					<td>{{eli.id}}</td>
					<td>{{eli.mailbox}}</td>
					<td>{{eli.status == 1?"已处理":"未处理"}}</td>
					<td>{{eli.date_added}}</td>
				</tr>
	    	{% endfor %}
		</tbody>
	</table>
</form>
```

再次测试,发现数据可正确传输到后台,说明第一种方式有问题,第二种方式是正确的.

那么是什么造成这种问题呢?都是get方式提交啊?

具体原因网上没找到标准答案,不过猜测是表单get提交应该不会管url里?后面的查询字符串.网上看到了类似的问题https://www.cnblogs.com/lz2017/p/7800114.html 先挖个坑,知道这样不能用就行了


