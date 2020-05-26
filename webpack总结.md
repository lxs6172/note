## <center>webpack</center>

### webpack是一种前端资源构建工具，一个静态模块打包器
构建工具？

1. 具有不同功能的小工具的集合，例如在html中引入less,但是需要编译成css样式才可以使用，这就需要一个编译工具，构建工具就是具有这些编译功能的工具的集合。

静态模块打包器

1.  每一个项目都有一个入口文件例如index.js，入口文件内包含引入项目中不同类型的资源，例如less/sass/jquery等等
2. 上述资源在页面中通过依赖的先后顺序，依次引入，形成了chunk（代码块）
3. 代码块进行依次处理，less变为css,jquery变为浏览器识别的js的语法，这个过程称为打包
4. 打包成功后将资源输出，输出的文件叫bundle


webpack的五个核心概念

1. Entry:入口指示，webpack以哪个文件为入口起点开始打包，分析构建内部依赖图
2. Output：输出指示webpack打包后的资源bundle输出到哪里，如何命名
3. Loader：让webpack去处理那些非javascript（webpack自身只理解JavaScript)，例如将css处理成webpack能识别的资源
4. Plugins：插件可以用于执行范围更广的任务，插件的范围包括从打包优化和压缩，一直到重新定义环境中的变量等，loader相当于是翻译成webpack能识别的资源，功能跟强大的需要plugins
5. Mode：模式分为两种development(开发模式)和production(生成模式)，development能让代码本地调试，运行的环境。production能让代码优化上线，运行的环境

