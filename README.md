perichr.js
==========

自用基础js库

基本用法

```html
<script src="/js/perichr.js" data-PK="787BBFDC" data-options="{&quot;opt1&quot;:&quot;value1&quot;,&quot;opt2&quot;:&quot;value2&quot;}" data-init="demo1.js,demo2.js" ></script>
```

目前文件名必须是`perichr.js`。
`data-PK`是项目号，用于缓解不可控环境下引入多个`perichr.js`时造成的地址冲突。需要和模块中的`key`属性配套使用。在安全可控环境下，可以省略此选项（模块中也同步省略`key`属性）。
`data-init`是后续载入的脚本列表，逗号分隔。使用`perichr.js`所在目录的相对位置。无此选项则自动载入`p.js`。
`data-options`数据要求为标准JSON格式化文本串，当后续载入的脚本是以约定的方式编写的话可以调用到这些数据。

这里有个简单的模块示意：

```javascript
P({  //这个是由perichr.js暴露的公开函数，用于模块的初始化。
    id: 'p.js',
    key: '787BBFDC',//项目号，缓解多perichr.js冲突
    Init: function(me){ //传入的me对象为主要操作对象，所有的可用特性都在这里。
        me.Load('p2.js', function(){ //
            console.log('p2.js已经载入，p.js准备做点啥了……')
            var p2 = me.GetPlugin('p2.js') 如果p2.js也是模块，则可以这样载入p2的me对象。
        })
	console.log(me.option.opt1)  //返回`undifined`。不能这样获取之前在`data-options`中定义的属性，此处option是为p.js单独设置的，目前还是空的。
	console.log(me.GetOption('opt1'))  //返回`value1`。用这个就可以了。会先查找`demo1.option`中的值，undifined时再检查`data-options`。
	console.log(me.option('opt1'))  //返回`value1`。这样也可以。
	me.option('opt1', 'val1')   //这样设置值。等同于`me.option.opt1 = val1`。
	console.log(me.option('opt1'))   //这时返回`val1`。
	
	me.GetFn('trim')('   1   ')       //调用perichr.js中公开的函数。
	me.fn('trim')('   1   ')       //这样也可以。
	me.fn.trim('   1   ')       //这样也可以。因为实际上已经把公开函数都复制了一遍到fn下。
	me.fn('trim', function(){})  //定义自己的函数。
	me.fn('trim', false)  //恢复默认的函数（如果有）。
    }
})

```
