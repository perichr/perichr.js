perichr.js
==========

自用基础js库

基本用法
```
<script id="_perichr_js_" src="/js/perichr.js" data-options="{&quot;opt1&quot;:&quot;value1&quot;,&quot;opt2&quot;:&quot;value2&quot;}" data-init="demo1.js,demo2.js"></script>
```
目前`id="_perichr_js_"`是必须的，且文件名必须是`perichr.js`。
`data-init`是后续载入的脚本列表，逗号分隔。使用`perichr.js`所在目录的相对位置。
`data-options`数据要求为标准JSON格式化文本串，当后续载入的脚本是以约定的方式编写的话可以调用到这些数据。

这里有个简单的模块示意：
```
(function(root, doc, perichr, undefined) {
	var demo1 = {  //一个包含了约定属性的对象，包括一个id指示和Init初始化函数。
			id: 'demo1.js', //必须用文件名作为id
			Init: function() {
				console.log('demo.js初始化中……')
			}
		}
	root[perichr].Load(demo1)  //将对象提交给由perichr.js公开的接口进行处理。对象的属性中将增加options和fn对象。
	
	console.log(demo1.option.opt1)  //返回`undifined`。不能这样获取之前在`data-options`中定义的属性，此处option是为demo.js单独设置的，目前还是空的。
	console.log(demo1.GetOption('opt1'))  //返回`value1`。用这个就可以了。会先查找`demo1.option`中的值，undifined时再检查`data-options`。
	console.log(demo1.option('opt1'))  //返回`value1`。这样也可以。
	demo1.option('opt1', 'val1')   //这样设置值。等同于`demo1.option.opt1 = val1`。
	console.log(demo1.option('opt1'))   //这时返回`val1`。
	
	demo1.GetFn('trim')('   1   ')       //调用perichr.js中公开的函数。
	demo1.fn('trim')('   1   ')       //这样也可以。
	demo1.fn.trim('   1   ')       //这样也可以。因为实际上已经把公开函数都复制了一遍到fn下。
	demo1.fn('trim', function(){})  //定义自己的函数。
	demo1.fn('trim', false)  //恢复默认的函数（如果有）。

	demo1 === root[perichr].Plugins['demo1.js']  //返回true。实际上可以通过root[perichr].Plugins来查找已经正确载入的其他模块。
	
})(window, document, '_perichr_')
```
