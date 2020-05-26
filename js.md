## js变量赋值问题
`http://hellobug.github.io/blog/javascript-variable-assignment/`

## 用户名为手机号码/邮箱的验证

	var reg = /^1[34578][0-9]{9}/;//手机号码正则验证
      var regExp = /^\w+((.\w+)|(-\w+))@[A-Za-z0-9]+((.|-)[A-Za-z0-9]+).[A-Za-z0-9]+$/; //邮箱验证
      if (value === '') {
        	callback(new Error('请输入用户名'));
   	 } else if(!regExp.test(value)&&!reg.test(value)){
       	 callback(new Error('请正确填写手机号码和邮箱！'));
   	 }

## 本地存储json字符串
1. JSON.parse将字符串转化为json对象。
2. JSON.stringify将json对象转化为字符串,本地存储时，存储的为json字符串。

## object获取key值和value值
`https://blog.csdn.net/doulvme/article/details/80404798`
  `var data={a:1,b:2,c:9,d:4,e:5};`

1. `Object.keys(data)`获取data中的key值，a,b,c,d。
2. `data[key]`获取key对应的value值，1，2，3等。

## 正则表达式
   `https://juejin.im/post/5ac1f1106fb9a028be362731`
   `https://www.liaoxuefeng.com/wiki/1022910821149312/1023021582119488`
   
   * 正则表达式可以用字面量的方式进行创建，和数组不同的是，正则表达式是以斜杠进行包裹的
   
		1. \w：用于匹配字母，数字或下划线字符。
		2. \d：用于匹配从0到9的数字；
		3. 非匹配符号是^，例如：[^abc]就是匹配非abc字符的其它字符：
		4. \S用以匹配所有的非空白字符
		5. 在正则表达式中.表示所有的字符，不过这个所有要除去换行符和行结束符：
		6. 问号?代表匹配的字符最多1个，尽量少匹配：
		7. 星号*代表匹配的字符0个到无数个，尽量多的去匹配：
		8. 加号+代表匹配的字符至少1个，尽量多的去匹配：
		9. 匹配开头的字符使用符号^，要注意的是这个^和非匹配的^区别在于，匹配开头字符的^是在中括号外面的：
		10. 匹配结尾字符使用符号$：
		11. 用*表示任意个字符（包括0个），用+表示至少一个字符，用?表示0个或1个字符，用{n}表示n个字符，用{n,m}表示n-m个字符。
		12. ？！判断是否存在什么条件
		13. [abc]:查找方括号之间的任何字符。
		14. [^abc]:查找任何不在方括号之间的字符。 
		
* 方法

		1. test()方法用以判断一个字符串是否匹配该正则表达式的，参数为需要进行匹配的字符串，返回一个布尔值：	
		2. exec()方法用来查找字符串中是否有匹配该正则表达式的字符，并且返回一个匹配字符所组成的一个数组，	

		
		
		
## js截取字符串常用方法
`http://www.hangge.com/blog/cache/detail_1887.html`

1. split方法，将截取的数据方放到一个数组中，使用一个指定的分隔符把一个字符串分割存储到数组。
2. substring(start，end)方法截取字符串	,start为从索引为几的值处截取，end为截取到end代表的数字的前一位。
3. substr(start，length)，start为开始截取处，数字为负数时，从后面查找到第几位，length为截取的长度。 	


## js中操作数组的常用方法
1. join(),返回一个字符串
   
        var a=[1,2,3];
        var b=a.join('');//b为字符串‘123’
   
2. reverse()方法，返回逆序数组
3. sort()方法，返回排序后的数组，如果数组包含undefined，会被排到数组的尾部。如果不带参数的调用sort()，数组元素以字母表顺序排序。
4. concat(),创建并返回一个新数组，将两个数组合并为一个数组。
5. slice()方法，返回指定数组的片段或者子数组。不会改变原数组，可以用来切割字符串，方法有两个参数，start为从索引值为start数值处开始切割，end为从索引值为end数值的前一个结束。
6. splice()方法，用来删除或插入元素，会修改原数组！
 
    	var a = [1,2,3,4,5,6,7,8];     
		var b = a.splice(1,2); //第一个参数是截取的起始位置（包括），第二个参数是截取的个数，之后的参数就是添加在元数组的新值		             
		console.log(a); //返回[1, 4, 5, 6, 7, 8]
		console.log(b); //返回[2, 3]

7. push()方法与pop()方法
	* push()方法在数组的尾部添加一个或者多个元素，并返回数组的新长度。注意的是，改变的是原数组的值，返回的是新数组的长度。
	* pop()方法删除数组的最后一个元素，并返回它的删除值。也是改变原数组，返回的是删除的值。
8. unshift()方法与shift()方法
	* unshift()方法类似于push()不同的是，他是在数组头部添加，其他都一样
	* shift()方法则类比pop()方法。
9. toString()方法将每个元素转化为字符串，类似于不传参的join()方法。
10. filter()方法，返回的是调用数组的一个子集。过滤数组中符合条件的数据，组成一个新的数组。
   
 		var a = [1,2,3,4,5];    
		var b = a.filter(function (value) {
		    return value > 3
		})             
		console.log(b); //返回[4,5]	
			
11. every()方法是只有数组中所以元素都满足某个条件才会返回true；some()方法是只要有满足条件的值，就返回true。
   
		var a = [1,2,3,4,5];          
		a.every(function (value) {
		    return value < 10
		}) //true		
		
		
## js !(非),||(或)
1. !(非)可将变量转换成boolean类型，null、undefined和空字符串取反都为true，其余都为false。
2. ||(或)如果一个为真，则为真，全部为假则为假，第一个判断为真，则后一个判断可以不用看总体为真，第一个判断为假，则看后一个判断的真假。
		

## map，reduce，filter，every，find
1. map把Array的所有数字转为字符串：

        var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
		arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
		
2. reduce() 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始缩减，最终为一个值。reduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce 的数组。

		var arr=[1,2,3,4,55];
		var total=arr.reduce(function(pre,next){
			return pre+next
		})
		<!--pre为初始值或上一次回调返回值，next当前数值-->

3. filter也是一个常用的操作，它用于把Array的某些元素过滤掉，然后返回剩下的元素。
和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

	三个参数依次为当前元素的值，当前元素的索引值，当前元素属于的数组对象
	
	indexof 某个指定字符串值在字符串中首次出现的位置

   * filter去重
   
		    var r,
		    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
		    r = arr.filter(function (element, index, self) {
			    return self.indexOf(element) === index; <!--返回当前数值在数组中首次出现的位置和当前数值的索引值相等的数据-->
			});

4. every()方法可以判断数组的所有元素是否满足测试条件。
5. find()方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回undefined：

## null和undefined的区别
undefined 表示一个变量没有被声明，或者被声明了但没有被赋值（未初始化），一个没有传入实参的形参变量的值为undefined，如果一个函数什么都不返回，则该函数默认返回undefined。 null 则表示“什么都没有”，即“空值”

1. null instanceof Object 结果为false
2. 

## Math.round

Math.round(1.49)四舍五入取整数，答案为1
  

## this的指向
`http://www.ruanyifeng.com/blog/2018/06/javascript-this.html`
`https://segmentfault.com/a/1190000008400124`

1. this最终指向的是调用它的对象

		