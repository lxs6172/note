## <center>webpack</center>

### webpack是一种前端资源构建工具，一个静态模块打包器
###构建工具？

1. 具有不同功能的小工具的集合，例如在html中引入less,但是需要编译成css样式才可以使用，这就需要一个编译工具，构建工具就是具有这些编译功能的工具的集合。

###静态模块打包器

1.  每一个项目都有一个入口文件例如index.js，入口文件内包含引入项目中不同类型的资源，例如less/sass/jquery等等
2. 上述资源在页面中通过依赖的先后顺序，依次引入，形成了chunk（代码块）
3. 代码块进行依次处理，less变为css,jquery变为浏览器识别的js的语法，这个过程称为打包
4. 打包成功后将资源输出，输出的文件叫bundle

###webpack的五个核心概念

1. Entry:入口指示，webpack以哪个文件为入口起点开始打包，分析构建内部依赖图
2. Output：输出指示webpack打包后的资源bundle输出到哪里，如何命名
3. Loader：让webpack去处理那些非javascript（webpack自身只理解JavaScript)，例如将css处理成webpack能识别的资源
4. Plugins：插件可以用于执行范围更广的任务，插件的范围包括从打包优化和压缩，一直到重新定义环境中的变量等，loader相当于是翻译成webpack能识别的资源，功能跟强大的需要plugins
5. Mode：模式分为两种development(开发模式)和production(生成模式)，development能让代码本地调试，运行的环境。production能让代码优化上线，运行的环境

### webpack环境的初始化
1. webpack init
2. 设置名称，其他选择默认值
3. sudo npm install webpack -g 
3. npm i webpack-cli -g    全局安装webpack-cli包
4. npm i webpack webpack-cli -D 本地安装webpack，webpack-cli
###运行指令？

1. 开发环境指令：webpack以src下的index.js为入口文件开始打包，打包后输出到build下的index.js中，整体打包环境是开发环境

		webpack ./src/index.js  o  ./build/index.js  --mode=development

2. 生产环境指令：webpack以src下的index.js为入口文件开始打包，打包后输出到build下的index.js中，整体打包环境是生产环境，生产环境的js会压缩大小，生产环境比压缩环境多一个压缩的js的代码

		webpack ./src/index.js  o  ./build/index.js  --mode=production

3. 生产环境和开发环境会将es6代码编译成浏览器可识别的模块化
4.  webpack能处理js/json文件，不能处理css/img

### webpack如何加载css样式？

1. 创建配置文件webpack.config.js
2. 配置项entry入口文件，output输出名称，输出路径，loader解析css的配置，不同的文件必须配置不同的loader，plugins配置，mode模式配置，暂选测试环境，容易查看打包后的文件
3. 所有的构建工具都是基于node.js平台运行的，模块化默认采用common.js

		// path  node.js的模块化，resolve为path模块化中的方法，用来拼接绝对路径
		const {resolve}=require('path')
		module.export={
		  entry:'./src/index.js',
		  output:{
		    //   输出名称
		      filename:'build.js',
		    //   输出路径
		    // _dirname  node.js的变量，代表当前文件的目录绝对路径，build表示在dirname下的那个文件夹
		      path:resolve(_dirname,'build')
		  },
		  moudle:{
		      rules:[
		        //   详细的loader配置
		        {
		            // 匹配哪些文件
		            test:/|.css$/,
		            // 使用哪些loader进行处理
		            // use执行顺序有下至上
		            use:[
		                'style-loader',//创建style标签，将js中的样式资源插入其中，添加到header中
		                'css-loader',//将css文件通过commin.js模块加载到js中，里面内容是以样式字符串形式存在的
		            ]
		        }
		      ]
		  },
		  puligns:{},
		//   mode:
		}
		
		不同的文件配置不同的loader，例如less,
		user的内容为'style-loader','css-loader','less-loader'
		
### webpack打包html资源，图片资源
1. 配置webpack.conf.js文件
2.  引入插件 npm i html-webpack-plugin -D 
3.  配置文件详细配置

		const {resolve} =require('path')
		const HtmlWebpackPlugin=require('html-webpack-plugin')
		module.exports={
		    entry:'./src/index.js',
		    output:{
		        filename:'build.js',//输出的文件名称
		        path:resolve(__dirname,'build')//输出文件放入的路径
		    },
		    module:{
		        rules:[
		            // 配置loader
		            {
		                // 匹配哪些文件
		                test:/|.less$/,
		                // 使用哪些loader进行处理,多个loader使用use
		                // use执行顺序有下至上
		                use:[
		                    'style-loader',//创建style标签，将js中的样式资源插入其中，添加到header中
		                    'css-loader',//将css文件通过commin.js模块加载到js中，里面内容是以样式字符串形式存在的
		                    'less-loader'//将less文件编译成css文件
		                ]
		            },
		            // 处理图片资源
		            {
		                // 此方法默认不能处理html中引入的图片
		                test:/\.(jpg|png|gif)$/,
		                // 下载url-loader,file-loader
		                loader:'url-loader',
		                options:{
		                    // 图片大小小于8kb，就会被base64处理
		                    // 优点：减少请求数量（减轻服务器压力）
		                    //缺点：转化后图片的体积会变大（文件请求速度变慢）
		                    limit:8*1024,
		                    // 问题：url-loader默认使用es6模块化解析，而html-loader引入图片是common.js
		                    // 解决：关闭url-loader的es6模块，使用common.js解析
		                    esModule:false,
		                    // 输出图片名称重新命名
		                    name:'[hash:10].[ext]'
		                }
		            },
		            // 处理html中引入的图片
		            {
		                test:/\.html$/,
		                // 负责引入img，从而使url-loader处理他
		                loader:'html-loader'
		            }
		        ]
		    },
		    plugins:[
		        // 插件，需要下载引入使用
		        // html-webpack-plugin插件功能：默认创建一个html，自动引入打包输出的所有资源（js/css)
		        new HtmlWebpackPlugin({
		            // template复制一个'./src/index.html'文件，并将输出的js/css引入其中
		            template:'./src/index.html'
		        })
		    ],
		    mode:"development",
		}