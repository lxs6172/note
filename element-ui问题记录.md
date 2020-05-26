# <center>Element-ui</center>
## 表单验证问题
+ rules 属性传入约定的验证规则，prop属性内写入需验证的字段名。
+ 红色星号为在rules中设置required: true,
+ status-icon属性为输入框添加了表示校验结果的反馈图标，直接在el-for 标签内添加即可。
+ this.$refs[formName].validate((valid){})为获取表单的内容，ref为标识，在el-for 内的ref 的名称一致。

## vue中引入element-UI
1. 安装 npm i element-ui -S
2. 在main.js中引入element-ui 全局使用

		   import ElementUI from "element-ui"
		   import 'element-ui/lib/theme-chalk/index.css'
		   
3. select 下拉框点击事件使用@click.native = 'xx()'

## v-for 循环注意点
  循环时要加入key值
`<el-col  :span="6" v-for="(items,index) in item" :key="items">`


## image组件，图片循环显示处理
+ vue项目引入图片方式
  1. img src="路径"，
  2. img :src="",此种引入方式需要使用require和import引入页面，使用
  3. import imgurl from "路径"，则imgurl则为图片路径
  4. require("路径")
  
+ 循环处理方式，使用require，require引入为一个变量
+ 不能为一个变量，需要使用拼接的方式，引入使用
+ 例如：

		this.url=require(`@/assets/image/${rout}.${(item.split('/')[3]).split('.')[1]}`);  
		
+ 如果为一个变量引入，则会报错找不到对应模块
		

