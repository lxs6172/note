## es6
[TOC]

### let和const命令

+ es6作用域用{}进行隔离，为一个作用域。
+ var 为函数作用域，let,const为块级作用域。
+ var 可以重新赋值，重复声明，有变量提升，在函数中声明时，可能为局部变量，在if等判断中定义，为全局变量。
+ let定义变量，和var相似，但是只在作用域内有效，不存在变量提升。
+ let 不允许在同一个作用域内重复定义同一个变量，变量可以重新赋值
+ const为定义常量，它的值被设置完成后就不能在修改了。但是对象中的值是可以改变对象中的某一个属性值。const不能重新赋值
+ const定义为不可修改的值，const jelly=Object.freeze(person)
+ let,const代替立即执行函数，立即执行函数将变量私有化。

#### 典型例子
1. 将var 修改为let时，输出则为0-9

       for(var i=0;i<10;i++){
          console.log(i);   // 0-9
          setTimeout(function(){
              console.log(i)  //10个10
          },1000)
      }
2. 变量提升问题

      console.log(color);//undefined
      	 var  color='yellow';		
       上述代码等价于

	    var color;
	    console.log(color);//undefined
		color='yellow';	 

  上述例子为变量提升，当将var 修改为 let 时，会报错referenceError				
### 箭头函数
1. 箭头函数特点：
   + 去掉function，不需要function关键字来创建函数
   + 一个参数（）可以省略，多个参数不可以省略，没有参数要加括号（）

     	const numbers=[1,2,3,4,5]
     	const double=numbers.map(function(number){
     	      return number*2
     	})
     	console.log(double)	

   上述代码修改为箭头函数

          const numbers=[1,2,3,4,5]
       	const double=numbers.map((number)=>{
       	      return number*2
       	})
       	console.log(double)	

2. 箭头函数，隐式返回，去掉return，删除花括号{}

     ```
     const numbers=[1,2,3,4,5]
     const double=numbers.map((number)=>number*2)
     console.log(double)	
     ```

3. 箭头函数是匿名函数

4. 箭头函数this指向问题
   + 箭头函数时，this指向父级作用域的this值。	

5. 箭头函数不宜使用情况 ：
   + 作为一个构造函数，一个方法需要绑定到对象
   + 当你真的需要this的时候。
   + 在函数中使用arguments对象时	

### 模版字符串

1. 将字符串放入反引号（``）内，包含起来，并将变量放入'${}'中。
2. 字符串函数
   + .startsWith():大小写敏感，表示参数字符串是否在原字符串的头部。
   + .endsWith():表示参数字符串是否在原字符串的尾部
   + .includes():某个值是否在字符串中，表示是否找到了参数字符串
   + .repeat():将字符串重复多次，传入的参数，为重复的次数
3. 字符串函数示例：

      	id='51030019800730366';
      	id.startsWith("51")//true
      	id.startsWith("1980",6)//6	表示开始搜索的位置

### 解构赋值
1. 快速的从对象中提取出属性。
2. 当变量名重复时，可以重新定义一个变量，也可以设置一个默认值。 
3. 对象解构赋值示例

          const Tom={
            name:'Tom Jones',
            age:25,
            family:{
                father:'Norah',
                mother:'Richard',
                borther:'Howard',
             }
          }
          const father='dad';
          const {father:f,mother,borther,sister='lily'}=Tom.family;
          console.log(f)//Norah
          console.log(sister)
4. 数组解构赋值
  + 注意：...为剩余数据，要放在最后使用	

      ```
      const numbers=['one','two','three','four','five'];
      const [one, , three, ...other ]=numbers;
      console.log(one,three,other)
      ```

  + 解构赋值，数组数据交换

      ```
      const a=10;
      const b=20;
      [a,b]=[b,a];		
      ```
### for of 循环
1. forEch 简化了语法，循环不可以终止循环或跳过循环。

2. for 循环比较繁琐，可读性低  

3. for in  循环，遍历的是所有可枚举属性，遍历出的是索引值，不是属性值

4. for of 循环遍历出的是属性值，支持终止循环和跳过

   	let array=['a','ab','abc','abcd'];
   	for(arr of array){
   	    console.log(arr)
   	}
   	
   	输出值：a ab abc abcd 
   	当of改为in时，输出结果为0，1，2，3

### Array.from,Array.of
1. Array.from将类数组转化为数组，通过`_proto_`可以查看是什么类型（对象，类数组，数组等）。

### map
* map通过key值获取value

      var map = new Map();
      map.set("pages/release/release", ["ModuleShow", "pages/release/release"]);//添加新的key-value
      
      map.get("pages/release/release")//获取"pages/release/release"的值

* map循环获取数组的每一条

        arr.map((item)=>{
              console,log(item)
          })    
* 定义map 

        mapRegion: new Map()
    ​      
### 类(class)
+ 在es6中，我们的javascript也有了类(class).
+ 类（class）通过 static 关键字定义静态方法。不能在类的实例上调用静态方法，而应该通过类本身调用。
+ static 语法

    `static methodName() { ... }`

 静态方法调用同一个类中的其他静态方法，可以使用this关键字
+ 非静态方法中，不能直接使用this关键字来访问静态方法，需要使用类名调用，className.staticName()   

### Array.from,includes,find,
1. 数组去重

   ES6中Array新增了一个静态方法Array.from，可以把类似数组的对象转换为数组

​       `let array = Array.from(new Set([1, 1, 1, 2, 3, 2, 4]));` 

2. includes方法可用来确定一个字符串是否包含在另一个字符串中。

   `let tag=this.roleList.every(val => this.tabRoleList.includes(val));`

   tips: 判断this.tabRoleList中是否包含this.roleList中的某个值，结果为true和false。

3. ES6为Array增加了find()，findIndex函数。
   * find()函数用来查找目标元素，找到就返回该元素，找不到返回undefined。
   * findIndex()函数也是查找目标元素，找到就返回元素的位置，找不到就返回-1。
   * 他们的都是一个查找回调函数。  

            const arr1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
              var ret1 = arr1.find((value, index, arr) => {
            	  return value > 4
              })
              value：每一次迭代查找的数组元素。
              index：每一次迭代查找的数组元素索引。
              arr：被查找的数组。
              
### promise 一个构造函数
1. Promise的构造函数接收一个参数，是函数，并且传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。
2. 当我们在 promise对象中成功时调用reslove函数，它会触发then方法中的第一个函数，当我们在 promise对象中成功时调用reject函数，它会触发then方法中的第二个函数，另外then中的第二个方法我们可以省略。我们可以使用 catch 来接受一些错误信息。

### async,await的使用

	`https://segmentfault.com/a/1190000015488033`
	`https://www.cnblogs.com/SamWeb/p/8417940.html`

1. 单独使用async与普通函数没有什么区别
2. async返回的是一个promise对象，需要使用.then()/.catch()的方法返回值
3. await需要在async内使用，await 就是异步等待，它等待的是一个Promise，因此 await 后面应该写一个Promise对象，如果不是Promise对象，那么会被转成一个立即 resolve 的Promise。 
4. async 函数被调用后就立即执行，但是一旦遇到 await 就会先返回，等到异步操作执行完成，再接着执行函数体内后面的语句。
5. 值得注意的是， await 后面的 Promise 对象不总是返回 resolved 状态，只要一个 await 后面的Promise状态变为 rejected ，整个 async 函数都会中断执行，为了保存错误的位置和错误信息，我们需要用 try...catch 语句来封装多个 await 过程