## post提交的数据编码格式踩坑

在做一个登陆功能的时候，发现参数传的明明是对的，但返回结果就是有问题，查原因时发现浏览器控制台显示提交的数据格式是下面这样的：

![post data format](https://github.com/LilyLaw/bug-records/blob/master/img/postdata.png?raw=true)

代码如下：
``` javascript
import axios from 'axios';

axios.post(param.url, param.data)
	.then(function (res) {
		console.log(111,res);
		// 请求成功，返回用户数据
		if(0===res.status){
			typeof resolve==='function' && resolve(res.data,res.msg);
		}
		// 没有登录，强制登录
		else if(10===res.status){
			this.doLogin();
		}else{
			typeof reject==='function' && reject(res.msg || res.data);
		}
	})
	.catch(function (err) {
		typeof reject==='function' && reject(err.statusText);
	});
```

发现传递数据的格式是 `json`

直觉感觉是数据传输格式出了问题（突如其来的诡异直觉，我也不知道怎么冒出来的。。。(⊙﹏⊙)b ）

看了下面的几篇文档：
[post提交数据的四种编码方式](https://www.jianshu.com/p/3c3157669b64)
[post提交的数据几种编码格式](https://www.cnblogs.com/zuobaiquan01/p/8414272.html)

大概是这个意思：

4种提交数据的格式，按需使用，上面代码之前提交的格式是没问题的，只不过跟后台服务处理数据的格式不对应

我提交的数据格式是```application/json```（axios默认提交方式是```application/json```） 而后台将传过来的数据按照 ```application/x-www-form-urlencoded```这个格式处理，所以取数据的时候就找不到，也就得不到想要的结果了。

这种情况下只要前端传参数的格式和后端处理参数的格式对应就ok了，改哪边按照自己需求来吧。

鉴于我用的别人的接口肯定不可能改后台了，所以只要把axios的默认传递格式给改成 和后台对应的```application/x-www-form-urlencoded```就可以啦！

代码如下：

``` javascript
import axios from 'axios';
import qs from 'qs';

axios({
		method:"post",
		url:param.url,
		headers:{
			'Content-type': 'application/x-www-form-urlencoded'
		},
		data:qs.stringify(param.data)
	})
	.then(function (res) {
		console.log(111,res);
		// 请求成功，返回用户数据
		if(0===res.status){
			typeof resolve==='function' && resolve(res.data,res.msg);
		}
		// 没有登录，强制登录
		else if(10===res.status){
			this.doLogin();
		}else{
			typeof reject==='function' && reject(res.msg || res.data);
		}
	})
	.catch(function (err) {
		typeof reject==='function' && reject(err.statusText);
	});
```

在此抛出以下几个疑问：
1. ```qs``` 是个啥？
2. https://www.jianshu.com/p/3c3157669b64  这个里面提出的那种方案我试了不行，是这个方案不行呢？还是axios本身有bug不完善呢？？？

