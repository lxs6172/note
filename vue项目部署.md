### 路由配置问题
`https://router.vuejs.org/zh/guide/essentials/history-mode.html`  

`https://blog.csdn.net/lmy0111ly/article/details/83055627`

`https://blog.csdn.net/qq_38337245/article/details/97518905`


### 项目打包，分为生产环境和测试环境

`https://blog.csdn.net/seanxwq/article/details/87099484  `
`https://www.jianshu.com/p/d4ffb5d2e2be`

1. vue-cli3.0 版本在新建的vue.Config.js中配置即可

   		 outputDir: process.env.NODE_ENV === "development" ? 'publicDist' : 'privateDist', // 不同的环境打不同包名

2. development为测试环境
3. 延伸：两个项目有大量的公共页面，只有登陆注册有区分，打包为两个包分别部署，可以假设其中一个项目为生产环境，一个为测试环境，生产两个包，去部署，在同一个路由中设置不同项目的登陆注册的显示路径不同，可以把路径设置为动态的。
4. 将两个包打成一样的文件，在.env.production和.env.development中设置NODE_ENV = 'production'，表示打包是在生产环境下完成的
5. 了解vue-cli环境变量和模式
`https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F`
6. 可以通过.env.xxx文件设置打包出来的包名称。	

### vue-cli3.0配置开发环境，测试环境，线上环境

直接使用axios.defaults.baseURL这中路径，需要后端进行跨域的处理

	`https://blog.csdn.net/qq_37055675/article/details/85047451`
	
	axios.defaults.headers.common['Authorization'] = store.state.token?	store.state.token:localStorage.getItem('token');
	/*第一层if判断生产环境和开发环境*/
	console.log("process.env.NODE_ENV",process.env.NODE_ENV)
	if (process.env.NODE_ENV === 'production') {
	   /*第二层if，根据.env文件中的VUE_APP_FLAG判断是生产环境还是测试环境*/
	   console.log("process.env.VUE_APP_FLAG",process.env.VUE_APP_FLAG)
	   if (process.env.VUE_APP_FLAG === 'pro') {
	       //production 生产环境
	       axios.defaults.baseURL = 'http://10.8.3.243:8082';
	
	   } else {
	       //test 测试环境
	       axios.defaults.baseURL = 'http://10.8.5.115:8082';
	
	   }
	 } else {
	   //dev 开发环境
	   axios.defaults.baseURL = 'http://10.8.5.184:8082';
	   // axios.defaults.baseURL = 'http://10.8.3.243:8082';
	 }
	 
### 使用proxy处理跨域问题

参考文章：https://juejin.im/post/5d1cc073f265da1bcb4f486d

代理处理跨域问题，测试环境可以，上线环境需要后端处理

1. vue-cli3版本，在vue.config.js中设置devServer

		devServer : {
	        proxy : {
	            '/index' : {
	                target : 'http://localhost/index',
	                // ws : true,
	                changeOrigin : true,
	                pathRewrite : {
	                    '^/index' : ''
	                }
	            }
	        }
	    }
成功获取数据后，我们访问的本来就是本地主机，只不过proxy转发了这个请求到一个新的地址。
* 首先，axios去访问/index/phpinfo.php，这是个相对地址，所以真实访问地址其实是localhost:8080/index/phpinfo.php
* 然而/index/phpinfo.php被我们配置的/index匹配到了 ，所以访问被proxy代理，那转发到哪个路径呢？在pathRewrite中，我们将模式^/index的路径清除了，所以最终的访问路径是 target+pathRewrite+ 剩余的部分 ， 这样也就是 http://localhost/index++/phpinfo.php



	
	
	
	
	
	
	
	
	
	
	
	
	
	 