* ###json字符串转换成对象
```
var object=$.parseJSON(data)
```

* ###删除数组中重复元素
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


