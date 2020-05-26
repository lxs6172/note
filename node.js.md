## <center>node</center>

### tips

1. 运行文件/项目，node 文件名/项目名
2. -dirname  输出当前目录的路径
3. -filename 当前文件的路径
4. 回调函数
   
	     function callFunction(fun,name){
	            fun(name);//回调fun，并将name作为参数
	     }
	     callFunction(function(name){
	           console.log(name + 'Bye');
	     },'rails365')//触发callFunction()函数，并传参，参数一为匿名函数，参数二为字符串
	     
### 定义模块，并使用
1. 新建一个count.js，并定义一个函数

	     var counter=function(arr){
		    return "There are"+  arr.length  + "elements in the array"
		}
		module.exports=counter;

2. 在count.js中暴露函数，并在app.js中导入函数，使用

       	var counter=require('./count')
		console.log(counter(['js','css','html','vue']))

3. 暴露多个函数或变量

 ` module.exports.counter={counter:counter};`		
			     