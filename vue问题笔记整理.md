# <center>vue框架</center>
## vue-cli3.0安装
1. 卸载2.9版本,报错的话，使用管理员权限（即加上sudo）

   `sudo npm install -g vue-cli`

2. 安装3.0版本，node版本需要高于8.9

   `npm install -g @vue/cli`
   
3. 3.0版本中没有了build，config文件夹，static中的文件可以放入到public中，配置文件为config.vue.js

4. 配置不同环境的打包文件。

   参考`https://blog.csdn.net/qq_36407748/article/details/82050976`
   在package.json中新建打包测试环境的命令
   
5. 创建新的vue项目
   
   `vue create XXXdemo`    
   
## vue 响应式原理（即数据变化页面发生变化的原因）
object.defineproperty(obj, prop, descriptor)方法的使用。 

1. obj为定义的属性对象（即要修改的值）。 
2. prop要定义或者修改的属性值的名称（即obj内包含的属性）。
3. descriptor将被定义或修改的属性描述符，包括数据描述符和存取描述符，我们用到的是存取描述符。
4. 存取描述符是由getter-setter函数对描述的属性。描述符必须是这两种形式之一；不能同时是两者。
5. get/set为两个函数，get当访问该属性时，该方法会被执行，set属性值别修改时触发执行此函数。

响应式原理实现代码

	//html
	<div>
		<p id="name"></p>
	</div>
	//js
	var obj={};
	object.defineproperty(obj, "name",{
		get(){
			return document.querySelector("#name").innerHtml;
		},
		set(val){
			document.querySelector("#name").innerHtml=val;		}
	})
	obj.name='xxx'
	
##### 总结：vue数据绑定的原理（数据劫持），利用object.defineproperty属性将data中的数据，每一个属性都定义了getter和setter，通过getter和setter通知需要更新的地方发生改变。

## 模拟数据
1. 在src同级新建mock文件夹，将模拟的json 数据放入其中。
2. 在build -> webpack.dev.conf.js内添加如下代码
       
		const express = require('express')
		const app = express()//请求server
		var appData = require('../mock/testv11.json')//加载本地数据文件
		var result = appData.data//获取对应的本地数据
		var apiRoutes = express.Router()
		app.use('/api', apiRoutes)//通过路由请求数据
3. 在watchOptions下添加如下代码，/api/result即为接口：
 
	     before(app) {
	      app.post('/api/result', (req, res) => {
	        res.json({
	          errno: 0,
	          data: result
	        })//接口返回json数据，上面配置的数据result就赋值给data请求后调用
	      }),
	      
4. 使用axios
 
		 axios.post('/api/result',{    
	        "MainKey":"StreamUserBoard",
	        "OwnerId":"1001000000013",
	    },{
	        "headers": {"Content-Type": "application/x-www-form-urlencoded"}
	    }).then((result)=>{
	        console.log("result",result);
	    })    
  	
		
## vuex
同一个页面不同组件的数据流通,集中的状态管理。

1. 安装vuex,并挂在vue上 Vue.use(vuex)
2. strict:true;//开启严格模式
3. state状态，唯一的数据源。只能在mutations中修改状态。
4. mutations(state,参数2)更改vuex中store中状态的唯一方法是提交mutations，修改全局变量必须通过mutations。

   `store.commit(函数名)`
   
5. actions//可以异步执行，不能直接修改全局变量，需要调用commit方法，触发mutation中的的方法。组件发生变化时，通过dispatch提交actions，actions通过提交commit，改变mutations的状态。
6. getters 派生出一些新的状态。重新定义一个状态值，状态值通过getters取得。
7.  modules将vuex的store对象分割成模块。

在src下，新建store文件夹，——>新建index.js ,引入vuex，并暴露出store。在main.js中引入store即可。

    import Vue from 'vue'
	import Vuex from 'vuex'
	
	Vue.use(Vuex)
	
	const store = new Vuex.Store({
	  modules: {
	    app,//分为app，user两个模块，每个模块都有每个模块拥有自己的 state、mutation、action、getter
	    user
	  },
	  getters
	})
	
	export default store
	
	
## 登陆问题：
+ 过程
   1. 第一次登陆，前端调用后端接口，并传参，参数为用户名和密码，
   
		    submitBtn() {
		      this.$refs['form'].validate((valid) => {
		        if (valid) {//验证通过
		          console.log("valid123",valid)
		          this.$store.dispatch("Login", this.user).then(response => {
		           调用store内actions的Login()函数将用户名，密码
		            if(!response || response.hasOwnProperty('ErrorCode')){
		              this.$message({
		                message: '登录失败',
		                type: 'fail',
		              });
		            }else{
		              this.$message({
		                message: '登录成功',
		                type: 'success',
		              });
		              this.$store.dispatch("Menu", response.Role.Menus)
		              this.$router.push({
		                path: this.redirect || '/business/application'
		              })
		            }
		          })
		        }
		      });
		    },
		    
	2. store内的user中的Login()函数，发送给后端，后端进行验证，并返回成功或失败的数据
	  
			    const user = {
				  state: {
				    token: getToken(),
				    name: '',
				    avatar: '',
				    roles: [],
				    Menu: []
				  },
				
				  mutations: {
				    SET_TOKEN: (state, token) => {
				      state.token = token
				    },
				    SET_NAME: (state, name) => {
				      state.name = name
				    },
				    SET_AVATAR: (state, avatar) => {
				      state.avatar = avatar
				    },
				    SET_ROLES: (state, roles) => {
				      state.roles = roles
				    },
				    SET_MENU: (state, Menu) => {
				      state.Menu = Menu
				    }
				  },
			    Login({commit}, userInfo){
			      return new Promise((resolve, reject) => {//返回一个promise对象，resolve成功的函数，reject（）失败的函数
			        login(Chicken.defaultOwnerId, userInfo).then(res =>{
			          setToken('token')  //设置值为token，名为TokenKey的cookie
			          commit('SET_TOKEN', 'token')//改变SET_TOKEN的值，即改变getToken()的值，getToken()为获取名为TokenKey的cookie
			          resolve(res)
			        }).catch(error =>{
			          reject(error)
			        })
			      })
			    },
			    
## router.beforeEach
+ router.beforeEach进入就是路由进入前的周期，同时有路由的来源和去向两个参数，可以判断和控制当前路由的走向和重定向。
+ 一般router.beforeEach配合vuex全局状态储存使用，验证用户登录状态。也可以结合sessionStorage 和localStorage使用，原理相同。
	
	        router.beforeEach((to, from, next) => {
				  console.log(to);   // 即将要进入路由的对象
				  console.log(from); // 当前导航要离开的路由对象
				  console.log(next); // 调用next该方法，才能进入下一个路由
				  next();
            });
            
  + 例子：
           
            router.beforeEach((to, from, next) => {
			  console.log(getToken())
			  if (getToken()) {
			    if (to.path === '/login') {
			      next({ path: '/business/application' })
			    } else {
			      const menu = JSON.parse(localStorage.menu)
			      store.dispatch('Menu', menu)
			      next()
			    }
			  } else {
			    if (whiteList.indexOf(to.path) !== -1) {
			      next()
			    } else {
			      next(`/login?redirect=${to.path}`) // 否则全部重定向到登录页
			    }
			  }
			})
   
## cookie
#### cookie写在客户端的一个文件，里面包含了你的登陆信息，token判断你是否授权登陆。
   1. cookies.set('name','属性') 设置cookie
   2. cookies.get('name')读取/获取cookie
   3. cookies.remove('name')删除cookie
   
   
## vue-router 的重定向-redirect
   开发中有时候我们虽然设置的路径不一致，但是我们希望跳转到同一个页面，或者说是打开同一个组件。这时候我们就用到了路由的重新定向redirect参数。
   
   1. redirect基本重定向
   
      我们只要在路由配置文件中（/src/router/index.js）把原来的component换成redirect参数就可以了。我们来看一个简单的配置。
      
            {
			    path: '/',
			    component: login,
			    redirect: '/b',//重定向到路径为/b的页面上
			    name: 'Business',
			    hidden: true,
			    children: [{
			      path: '/b',
			      layout: HeaderAside,
			      component: Listsub, 
			    }]
			  },
			  
## layout 布局
##### 实际情况应该是大多数页面都依托于布局组件 Layout，里面有一个侧边栏 SideBar，主页面 AppMain，导航栏 NavBar

     <template>
		  <div class="app-wrapper" >
		    <side-bar class="sidebar-container"></side-bar>
		    <div class="main-container">
		      <nav-bar></nav-bar>
		      <app-main></app-main>
		    </div>
		  </div>
	</template>
	
+ 当我们点击 sidebar 时，应该是 sidebar 不变，app-main 的内容动态改变。这个时候刚才那个路由配置就不行了，应该上嵌套路由。
+ 具体就是像登录，错误页等一些单独的页面（一级路由），那就放在主页面 App.vue 中动态渲染，即在 App.vue 中添加 <router-view/> 标签。
+ 而像是用户权限管理等左侧菜单内包含的业务页面等，他们的布局都一致，也就是依托于布局组件 Layout （二级路由），那就把它们都交给 Layout 中的 app-main 取动态渲染。也就是在app-main.vue 中添加 <router-view/> 标签， Layout 和登录，错误页一样，在 App.vue 中渲染。
+ 从路由上写页面是在app.vue上显示，还是Layout中的appmain中显示
`https://panjiachen.github.io/vue-element-admin-site/zh/guide/essentials/layout.html#layout`

		// No layout
		{
		  path: '/401',
		  component: () => import('errorPage/401')
		}
		
		// Has layout
		{
		  path: '/documentation',
		
		  // 你可以选择不同的layout组件
		  component: Layout,
		
		  // 这里开始对应的路由都会显示在app-main中 如上图所示
		  children: [{
		    path: 'index',
		    component: () => import('documentation/index'),
		    name: 'documentation'
		  }]
	}


## vue生命周期，钩子函数
+ beforeCreate在实例初始化之后，数据观测和event/watcher事件配置之前被调用。此时，this变量还不能使用。
+ created实例创建完成后调用，在这一步中，完成了数据的观测，属性和方法的观测，但是挂载阶段还没开始，$el属性目前不可见。这个时候可以操作	vue实例中的数据和各种方法，但不能对dom节点进行操作。
+ mounted挂载完毕，这时dom节点被渲染到文档内，一些需要dom的操作在此时才能正常进行。
+ 先执行父组件的created和beforeMounted函数；再按子组件的使用顺序，执行子组件的created和beforeMounted函数；依旧按照子组件的执行顺序执行mounted函数，最后是父组件的mounted函数； 
也就是说父组件准备要挂载还没挂载的时候，子组件先完成挂载，最后父组件再挂载；所以在真正整个大组件挂载完成之前，内部的子组件和父组件之间的数据时可以流通的


## v-for 循环		
在elementui中需要写key的值

 `v-for="(jobs,item) in UserJobs" :key='item'`
 
for循环是判断最后一条，并添加不同的页面样式
1. 可以通过添加一个class类实现,判断index值和数组最后长度是否一致
   `:class="{'mysty':index === UserJobs.length -1}"`
2. 可以通过v-if判断,显示或隐藏
   `v-if="i===listdata.length-1"`
   
3. 链接 https://blog.csdn.net/qq_39109182/article/details/81064758

## 使用定时器实现进度条
setInterval定时器循环执行，不停止，setTimeout定时器，触发后只执行一次，可以使用回调函数循环执行，clearInterval为清除定时器，this.progressvalue为进度数值

     let aprogress=setInterval(() => {
        this.progressvalue++;
        if(this.progressvalue>100){
          clearInterval(aprogress);
          this.dialogTaskVisible = false;
          this.progressvalue=98;
          this.$message('作业执行成功');
        }
      }, 1000); 
      
## 父子组件传值
+ 父组件向子组件传值

  父组件：
  
	      <template slot-scope="scope">
	        <span style="margin-left: 10px;color:#409EFF;cursor: pointer;" @click="actionclick()">{{ scope.row.taskname }}</span>
	      </template>
	      <action-center :parentMessage="parentMessage"></action-center>
	      
  子组件：props子组件内接收父组件的传递的值
  
        <!--子组件-->
		<template>
		  <div>
		    <h3>我是子组件一</h3>
		    <span>{{parentMessage}}</span>
		  </div>
		</template>
		<script>
		  export default{
		    props: ['parentMessage'],
		  };
		</script>
  
        
+ 子组件向父组件传值

	    <!--父组件-->
			<template>
			  <div>
			    <h2>父组件</h2>
			    <br>
			    <Child-one @childEvent="parentMethod"></Child-one>
			  </div>
			</template>
			<script>
			  import ChildOne from './ChildOne';
			
			  export default{
			    components: {
			      ChildOne,
			    },
			    data() {
			      return {
			        parentMessage: '我是来自父组件的消息',
			      };
			    },
			    methods: {
			      parentMethod({ name, age }) {
			        console.log(this.parentMessage, name, age);
			      },
			    },
			  };
			</script>
子组件：通过`$emit`向父组件传值
			
		<!--子组件-->
		<template>
		  <div>
		    <h3>我是子组件一</h3>
		  </div>
		</template>
		<script>
		  export default{
		    mounted() {
		      this.$emit('childEvent', { name: 'zhangsan', age:  10 });
		    },
		  };
		</script>
		
		
## 父传子子孙孙
1. provide(){return {somevalue:"传给子孙后代的值"}}
2. 接受处：reject:["somevalue"]

## slot插槽
为了给子组件留地方，父组件的slot插槽处可以引入子组件。
		
## route路由传参（例如列表项进去详情页）
`https://segmentfault.com/a/1190000012393587`

1.  使用`name`路由的名称，params

        :to="{name:'TaskItemAdd',params:{ id:'item.id' }}"
        
    详情页接收id，此方法id不显示在URL中
    
        this.$route.params.id
        
    存在的缺点：进入详情页，再次刷新详情页的页面，会报数据不存在的错误
    
    解决办法：在路由配置中，name上的path路径后添加上`/id`名即可
    
         {
	        path: '/Job/Action/taskItemAdd/:id',
	        name: 'TaskItemAdd',
	        layout: Layout,
	        component: TaskItemAdd,
	      },
	      
2.  使用`path`路径和query
  
        :to="{path:'/Job/Action/taskItemAdd',query:{ id:'item.id' }}"
        
     详情页接收id，此方法id会显示在URL路径中
    
        this.$route.params.id 
        
3. 路由传参的基本方法：
   
        this.$router.push({
          path: `/describe/${id}`,
        })
       
   此方法在路由中需要配置信息：
   
         {
		     path: '/describe/:id',
		     name: 'Describe',
		     component: Describe
		   }

 ## 点击空白部分，关闭编辑框
 
 `https://blog.csdn.net/qq_29436563/article/details/79994388`
 
 页面代码：
 
	     <el-input  v-model="argumentInput"
	         v-show="citem===item&&cindex===index&&ci===i&&!editValue"
	         v-clickoutside="handleClose"
	         @change="handlePost(item,index,i,
	         argument.split('=')[0],argumentInput),'1'">
	     </el-input>
                        
                        
 指令：
 
        const clickoutside = {
	       // 初始化指令
		    bind(el, binding, vnode) {
		      function documentHandler(e) {
		        if (el.contains(e.target) || e.target.localName === 'input')   
		           { return false; }
		        if (binding.expression) { binding.value(e); }
		      }
		      el.__vueClickOutside__ = documentHandler;
		      document.addEventListener('click', documentHandler);
		    },
		    update() {},
		    unbind(el, binding) {
		      document.removeEventListener('click', el.__vueClickOutside__);
		      delete el.__vueClickOutside__;
		    },
	    };
	

   ` directives: {
	    clickoutside
	  },`
	  
	  
		 handleClose(e){
	      // console.log("eeeeee",e.target.localName)
	      if(e.target.localName !== 'i'){
	        this.editValue = true ;
	        this.editOutValue=true;
	      }
	    }, 
	   	handlePost(item,index,i,key,Input,tag){
	      if(tag==0){
	        this.Roles[item].Steps[index].Outputs[i]=`${key}=${Input}`;
	      }else{
	        this.Roles[item].Steps[index].Arguments[i]=`${key}=${Input}`;
	      }
	      console.log("123456",this.Roles[item].Steps[index])
	    },
	    
	    
### 点击上传功能


	    <template>
	    <div class="addUpload">
	        <el-button type="primary" size="small" @click="addjobs">新增</el-button>
	        <!-- 新增作业 -->
	        <el-dialog
	                title='新增作业'
	                :visible.sync="dialogVisible"
	                width="700"
	        >
	            <div class="task-upload">
	                <label for="fileinp" id="fileselect" class="el-upload-dragger">
	                    <i class="el-icon-upload"></i>
	                    <input type="file" id="fileinp">
	                    <div id="text">{{filetext}}</div>
	                    <div class="el-upload__tip" slot="tip"><span>请上传json文件，且不超过500kb</span></div>
	                </label>
	            </div>
	            <span slot="footer" class="dialog-footer">
	                <el-button @click="dialogVisible = false" type='info'>取 消</el-button>
	                <el-button type="primary" @click="taskfile">确 定</el-button>
	            </span>
	        </el-dialog>
	    </div>
	</template>
	<script>
	export default {
	    name:'addUpload',
	    props: {
	
	    },
	    data () {
	        return {
	            dialogVisible: false,
	            filetext: '',
	        }
	    },
	  	 methods:{
	        // 新增上传
	        addjobs() {
	            this.dialogVisible = true;
	            this.$nextTick(function () {
	                this.readFile();
	            })
	        },
	        // 读取上传文件内容
	        readFile() {
	            let fileselect = document.getElementById('fileselect')
	            fileselect.addEventListener('change', function (e) {
	                let file = e.target.files
	                // console.log('文件类型',file)
	                this.filetext = e.target.files[0].name;
	                // console.log("filetext",this.filetext)
	                if (file.length === 0) {
	                    return
	                }
	                let reader = new FileReader()
	                if (typeof FileReader === 'undefined') {
	                    this.$message({
	                        type: 'info',
	                        message: '您的浏览器不支持FileReader接口'
	                    })
	                    return
	                }
	                reader.readAsText(file[0])
	                reader.onload = function (e) {
	                    localStorage.params = this.result;
	                }
	            }.bind(this))
	        },
	        // 确定上传文件
	        taskfile() {
	            this.dialogVisible = false;
	            // this.callback();
	            // this.Orchestrations();
	            this.$emit('parentMethod',localStorage.params)
	            this.$message({
	                message: '新增成功',
	                type: 'success',
	            });
	        },
	    },
	}
	</script>


### vue中使用键盘事件
 
* 没有使用element-ui框架下使用
  1. 在input标签内直接使用,submit为定义的函数
       
      `<input @keyup.enter="submit()">`
      
* 在element-ui框架中使用键盘事件
  1. 因为框架中的input是封装好的，所以需要加上native
  
      `<@keyup.enter.native"submit">`
      
  2. input框在from表单中，并且有且只有一个input框，会发生enter时页面进行刷新，因为from表单进行了提交。
  
     解决办法：1. 不使用from表单。
             2. 在from表单中添加一个input标签，并将其隐藏。
             3. element官方解决方案：在el-from 加上 @submit.native.prevent        
* 参考文章

  `https://segmentfault.com/a/1190000016034270`  
  `https://www.cnblogs.com/cristina-guan/p/9440035.html`               


### 父子组件传值时，子组件获取到的数据，刷新时为空，但是数组点开能看到？
  1. 原因：传输的数据为父组件异步获取的，子组件取到的值为空时，页面上子组件的部分就已经渲染了，即页面显示为空。
  2. 解决办法：在父组件引入子组件处进行v-if判断，判读值大于0时，即允许子组件显示。
  
  ` https://forum.vuejs.org/t/how-to-access-the-elements-of---ob---observer-in-vuejs/22404/4`
  
  
### vue中ob_observer的意思？
 1. Observer是一个内部Vue对象，它使数据具有反应性。你无需做任何事情。您可以像在javascript中那样获取或访问数据。您的数据上的私有财产没有任何变化。
 2. 相当于对数据设置的监控器。

 
### vue中ref的使用

* ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例。
* 关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。


1. 在父组件中直接访问获取子组件中定义的值，可以在子组件中使用ref.
 
	 	 <navbar ref="navbar"></navbar>//父组件中直接引用子组件
	 	 console.log(this.$refs.navbar.navs); //获取子组件中定义的值
 	 
2. 在页面的标签中使用，即可获取标签内的文本信息，这样就不需要给标签设一个id，再document.getElementById('xx)。

	    <p ref="thisP">{{name}}</p></div>
	    vm.$refs.thisP.textContent //获取p标签中的文本信息
	    
3. 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM.


	      this.$nextTick(()  => {
		    console.log(vm.$refs.thisP.textContent);
		  })	    
		  
### Vue.nextTick 的使用

`https://blog.csdn.net/zhouzuoluo/article/details/84752280`

1. 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
2. Vue 实现响应式并不是数据发生变化之后 DOM 立即变化，而是按一定的策略进行 DOM 的更新。
3. Vue 在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。
4. Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中，原因是在created()钩子函数执行的时候DOM 其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted钩子函数，因为该钩子函数执行时所有的DOM挂载已完成。
		  
### vue项目中v-for循环时，出现死循环问题
#### 解决办法		  
    `https://blog.meathill.com/category/js/vue`
    
  将调用的函数中使用的全局变量设置为局部变量，不放置在data中定义
		  
### vue项目中修改对象的对象，赋值修改之后不改变
#### 例如 

	data:{
	  Organization: {
	        OrganizationId: '',
	        OrganizationCNName: '',
	    },
	}
	
给	OrganizationId重新赋值时，不起作用
#### 解决办法：将对象复制一份重新赋值
https://blog.csdn.net/yihanzhi/article/details/74200618

    this.ruleForm.Organization=Object.assign({},this.ruleForm.Organization,{OrganizationId:this.ruleForm.Organization.OrganizationId});
    

### vue项目引入图片
1. import引入，在js中引入
2. 直接src ='',
3. require引入，引入的路径不能完全是一个变量，不然找不到
4. require引入失败原因
  	+ webpack本身是一个预编译的打包工具，无法预测未知变量路径 不能require纯变量路径
	+ require(path) ,path 至少要有三部分组成, 目录+文件名+后缀
	+ 目录 =>  webpack 才知道从哪里开始查找
	+ 后缀 => 文件后缀，必须要加上，不然会报错
	+ 文件名 => 可用变量表示
5. require引入解决办法

		// # 变量名加字符串 编译成功
		this.imgUrl='images/no-result.png'
		<img :src="require('@/assets/'+imgUrl+'.png')">


  
### 图片上传
+ 方法一：
	1. 后端提供一个servlet路径，用于读取和存储数据。
	2. 前端使用elementUI中upload组件即可，onSuccess 函数中会返回，上传成功之后，返回的接口
+ 方法二：
   1. 后端接受base64格式的路径，存储到数据库中。
   2. 前端进行，读取和上传数据，在读取返回路径。
   3. 例如：
        
	        const reader = new FileReader();
	        reader.readAsDataURL(file.raw);
	        reader.onload = function (e) {
	        	let dataUrl=(this.result).replace(/.*;base64,/, '');
	        	// dataurl 为返回的base64格式的路径 
	        };	
	        
### 监听本地缓存中的数据
`https://www.cnblogs.com/scale/p/10973069.html`

1. 在main.js中引入函数，为全局函数，判断key是否等于想要监听的数据，若是，则触发函数

		Vue.prototype.resetSetItem = function (key, newVal) {
		   	if (key === 'watchStorage') {
		       // 创建一个StorageEvent事件
		       var newStorageEvent = document.createEvent('StorageEvent');
		       const storage = {
		           setItem: function (k, val) {
		               sessionStorage.setItem(k, val);
		               // 初始化创建的事件
		               newStorageEvent.initStorageEvent('setItem', false, false, k, null, val, null, null);	
		               // 派发对象
		               window.dispatchEvent(newStorageEvent)
		           }
		       }
		       return storage.setItem(key, newVal);
			}
		}
		
2. 调用函数触发，将本地缓存中的值修改为最新值。
   
   this.resetSetItem("key",value);
   
3. 监听函数，数据发生变化，即触发函数。将想要修改的函数，改为上面出发时的值

	window.addEventListener("setItem", () => {
	
	})  
	
### 多级路由嵌套
####  例如三级路由对应页面的内容显示在根路由的显示页面内
1. 子路由显示在父路由的《router-view》里
2. routerConfig为定义的路由数组

		const recursiveRouterConfig = (config = []) => {
		  config.forEach((item) => {
		    const route = {
		      path: item.path,
		      component: item.layout,
		      name:item.name,
		      children: [
		        {
		          path: '',
		          component: item.component,
		          name:item.name,
		        },
		      ],
		    };
		
		    if (Array.isArray(item.children)) {
		      recursiveRouterConfig(item.children);
		    }
		    routerMap.push(route);
		  });
		
		  return routerMap;
		};
		
		const routes = recursiveRouterConfig(routerConfig);
		
		
		
		
		
		
		
		
		
		
		
		