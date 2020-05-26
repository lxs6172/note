# <center>fetch传数据</center>
## fetch在vue中的使用总结
fetch 是用来代替传统的XMLHttpRequest的，他的优点包括，链式调用的语法，返回promise
1.fetch post请求，文件为js文件

    fetch(url, {
            method: 'POST',
            body: JSON.stringify(json), 
            headers:{
                'Content-Type': 'application/x-www-form-urlencoded'            
            }
        })
        .then(data =>{
            return data.json();//获取后端传的数据
          })
        .catch(error => console.error('Error:', error));
2.在组件内引入js文件，并调用

 `import Organzation from '@/assets/js/Organization'`
 
 在组件函数中调用
     
     getOrganizations()
     //this.ownerId为向后端传递的参数
      Organzation.GetOrganizations(this.ownerId)       
      .then(res =>{
        this.organizations = res.Organizations
        this.getRoles();
      })
    },
 
 注意：
      
            Response 实现了 Body, 可以使用 Body 的 属性和方法:

            resp.type // 包含Response的类型 (例如, basic, cors).

            resp.url // 包含Response的URL.

            resp.status // 状态码

            resp.ok // 表示 Response 的成功还是失败

            resp.headers // 包含此Response所关联的 Headers 对象 可以使用

            resp.clone() // 创建一个Response对象的克隆

            resp.arrayBuffer() // 返回一个被解析为 ArrayBuffer 格式的promise对象

            resp.blob() // 返回一个被解析为 Blob 格式的promise对象

            resp.formData() // 返回一个被解析为 FormData 格式的promise对象

            resp.json() // 返回一个被解析为 Json 格式的promise对象

            resp.text() // 返回一个被解析为 Text 格式的promise对象
        

## 补充

1、fetch() 返回的是一个Promise对象。

　　fetch使用的promise对象可以使得我们使用同步的方式写异步函数。

 

2、 fetch api是可以结合 async 和 await 来使用的。 

　　fetch是基于promise实现的，但是使用promise的写法，我们还是可以看到callback的影子，如果结合 async和await来使用，还是非常不错的。

 

3、 Fetch api 提供的spi囊括但是不限于xhr的所有功能。

 

4、 fetch api 可以跨域。 


## git 问题
1. 初始化本地仓库

 `git init `
 
2. 上传文件步骤
   
	     git status //查询状态
	     git add. //提交到暂缓区
	     git commit -m "" //提交时的信息
	     git push //提交到gitlab上
	     
3. 解决服务器与本地冲突问题

	    git stash
	    git push origin master
	    git stash pop
	  
4. git 命令详细讲解

   `https://www.jianshu.com/p/53a00fafbe99`
    
    
5. git 删除已经上传的某一个文件
   
   `https://www.cnblogs.com/muxiaoyi/p/6257648.html` 
   
   		 git rm -r --cached  "XXXX文件名称" //--cached 只从索引区删除  ,-r 允许递归删除
   		 git add .
   		 git commit -m '删除xxx'
   		 git push
  tips:注意需进去到文件的上一级文件夹中删除。
  
6. 项目移到新的git 地址中，再次提交，怎么办
	
		https://blog.csdn.net/ApatheCrazyFan/article/details/80581104

    *  可以在webstorm中切换git 地址。vcs->git->remotes,修改地址即可
    *  若提示以下代码，则表示没有配置一个本地git仓库跟踪到远程git仓库，有两种方法
    
    		No tracked branch configured for branch master. To make your branch track a remote branch call, for example, git branch --set-upstream-to origin/master master

	   1. 按照提示 执行代码git branch --set-upstream-to origin/master master，在git push -u
	   2. 将修改的代码，提交到暂缓区，并commit记录一下，提交前先从远程仓库主分支中拉取请求 git pull origin master，把本地仓库代码提交git push -u origin master
  