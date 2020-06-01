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
	 
### vue的跨域问题

	https://segmentfault.com/a/1190000016199721

1. 前端在测试环境设置代理，进行跨域

		devServer: {
	        proxy: {
	            '/api':{
	                target: 'http://10.8.5.230:8082',//代理地址
	                changeOrigin:true,//是否跨域
	                pathRewrite:{  //发送对象前重写路径
	                    '^/api':'',
	                }
	            }
	        }
	    },
 * 	设置代理要有一个标识，告诉node，接口以'/api'开头的才使用代理
 *   	'^/api'，“^”匹配的是字符串最开始的位置，axios进行请求时，'/api/login'，在经过代理服务器时会变成/login
 *    路径重写，也就是说会修改最终请求的API路径。比如访问的API路径：/api/login,
 *    设置pathRewrite: {'^/api' : ''},后，最终代理访问的路径：http://10.8.5.230:8082/users，这个参数的目的是给代理命名后，在访问时把命名删除掉。
		
2. 设置nginx反向代理，部署项目到服务器上
	 
* nginx->conf->nginx.conf  配置反向代理
  
		server{
			location /{
				root  html;//root对应index.html对应的外层文件夹所在的位置
				index  index.html  index.htm;
			}，
			location /api {     //匹配前端配置的名称
				rewrite  ^.+api/?(.*)$  /$/ break; //重写路径，所有以api请假的接口
				proxy_pass http:10.8.3.247;//代理地址
				proxy_set_header Host $host; 
				proxy_set_header X-Real-Ip $remote_addr;
				proxy_set_header X-Forward-For $proxy_add_x_forwarded_for
			}
		}
	   
### 将项目部署到服务器上
`https://www.bilibili.com/video/BV1ZE41157FU?from=search&seid=4160680904972906499`
1. 连接服务器，终端-->shell-->新建远程连接-->ssh-->新增服务器地址，选中服务器连接
2. 上传打包文件

	终端-->shell-->新建远程连接-->文件传输-->选中服务器连接 ，输入命令 ,上传文件
	
	` put /Users/liuxuesong/Documents/云管2020/web/dist.zip /usr/local/nginx/prodist `
	
	  `/usr/local/nginx/prodist 要上传到的路径`
	
	`/Users/liuxuesong/Documents/云管2020/web/dist.zip压缩文件在本地的路径`
2. 找到打包后的文件要放置的位置 

		cd /usr/local/nginx/prodist

3. 删除prodist下的所有文件
	 `rm -rf *`
4.  解压文件 
	`unzip dist.zip`
	
5. 进入dist,复制dist内文件到上一层	
	`cp -r * ../`
6. 进入nginx文件夹，停止nginx,在启动
	`cd usr/local/nginx/sbin`
	`./nginx -s stop`
	`./nginx`
7. 进入nginx ，配置nginx.conf文件
  `vim nginx.conf`
  编辑 i，
  退去编辑并保存 esc->:wq
  强制退出不保存 esc->:q!
8. 配置nginx后，重新启动
	`./nginx -s reload`   	
	
### vue 项目部署的坑
`https://segmentfault.com/a/1190000016919340	`
`https://www.cnblogs.com/xiaofenguo/p/11290090.html`//路由动态刷新报错
	
	configureWebpack: {//通过操作对象方式，修改webpack配置
         output:{
            filename:`/js/[name]_[chunkhash].js`,
           chunkFilename:`/js/[name]_[chunkhash].js`
         },
      },	
	
	
	
	
	
	
	 