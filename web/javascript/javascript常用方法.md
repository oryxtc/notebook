---
title: javascript常用方法
date: 2017/1/14 20:46:25
categories:
- web
- javascript
tags: javascript
description: json字符串转换成对象&删除数组中重复元素&去掉字符串中转义和特殊字符...
---

### 1.json字符串转换成对象
```
var object=$.parseJSON(data)
```

### 2.删除数组中重复元素
```javascript
<script type="text/javascript">
var  a=[4,6,7,6,44,64,4];
function arr2(arr){
	for(var i= 0;i<arr.length;i++){
		var demo1=arr[i];
		for(var j=0;j< arr.length;j++){
			var demo2=arr[j];
			if(i!=j && demo1==demo2){
				arr.splice(j,1);
			}
		}
	}
return arr;
}
//测试
var res=arr2(a);
console.debug(res)
</script>
```

### 3.去掉字符串中转义和特殊字符
```javascript
var excludeSpecial=function(str) {
	// 去掉转义字符
	str= str.replace(/[\'\"\\\/\b\f\n\r\t]/g, '');
	// 去掉特殊字符
	str= str.replace(/[\@\#\$\%\^\&\*\{\}\:\"\L\<\>\?]/,'');
	return str;
};
```

### 4.手机类型判断
```javascript
var BrowserInfo = {
    userAgent: navigator.userAgent.toLowerCase()
    isAndroid: Boolean(navigator.userAgent.match(/android/ig)),
    isIphone: Boolean(navigator.userAgent.match(/iphone|ipod/ig)),
    isIpad: Boolean(navigator.userAgent.match(/ipad/ig)),
    isWeixin: Boolean(navigator.userAgent.match(/MicroMessenger/ig)),
}
```

### 5.返回字符串长度，汉子计数为2
```javascript
function strLength(str) { 
    var a = 0;
    for (var i = 0; i < str.length; i++) {
        if (str.charCodeAt(i) > 255)
            a += 2;//按照预期计数增加2
        else
            a++;
    }
    return a;
}
```

### 6.获取url中的参数
```javascript
function GetQueryStringRegExp(name,url) {
    var reg = new RegExp("(^|\\?|&)" + name + "=([^&]*)(\\s|&|$)", "i");
    if (reg.test(url)) return decodeURIComponent(RegExp.$2.replace(/\+/g, " ")); return "";
}
```

### 7.JS判断两个日期大小 适合 2012-09-09 与2012-9-9 两种格式的对比
```javascript
//得到日期值并转化成日期格式，replace(/\-/g, "\/")是根据验证表达式把日期转化成长日期格式，这样再进行判断就好判断了
function ValidateDate() {
    var beginDate = $("#t_datestart").val();
    var endDate = $("#t_dateend").val();
    if (beginDate.length > 0 && endDate.length>0) {
        var sDate = new Date(beginDate.replace(/\-/g, "\/"));
        var eDate= new Date(endDate.replace(/\-/g, "\/"));
        if (sDate > eDate) {
            alert('开始日期要小于结束日期');
            return false;
        }
    }
}
```

### 8.回车提交
```javascript
$("id").onkeypress = function (event) {
    event = (event) ? event : ((window.event) ? window.event : "")
    keyCode = event.keyCode ? event.keyCode : (event.which ? event.which : event.charCode);
    if (keyCode == 13) {
        $("SubmitLogin").onclick();
    }
}
```

