## <center>java</center>

### 安装idea 
1. 下载idea，在bin中放入破解文件
2. 修改破解文件中的路径
3. 输入注册码
4. 注册码如下：


    	ThisCrackLicenseId-{
			"licenseId":"ThisCrackLicenseId",
			"licenseeName":"web",
			"assigneeName":"",
			"assigneeEmail":"web@163.com",
			"licenseRestriction":"For This Crack, Only Test! Please support genuine!!!",
			"checkConcurrentUse":false,
			"products":[
			{"code":"II","paidUpTo":"2099-12-31"},
			{"code":"DM","paidUpTo":"2099-12-31"},
			{"code":"AC","paidUpTo":"2099-12-31"},
			{"code":"RS0","paidUpTo":"2099-12-31"},
			{"code":"WS","paidUpTo":"2099-12-31"},
			{"code":"DPN","paidUpTo":"2099-12-31"},
			{"code":"RC","paidUpTo":"2099-12-31"},
			{"code":"PS","paidUpTo":"2099-12-31"},
			{"code":"DC","paidUpTo":"2099-12-31"},
			{"code":"RM","paidUpTo":"2099-12-31"},
			{"code":"CL","paidUpTo":"2099-12-31"},
			{"code":"PC","paidUpTo":"2099-12-31"}
			],
			"hash":"2911276/0",
			"gracePeriodDays":7,
			"autoProlongated":false}

### idea 创建maven项目

参考如下链接：

`https://blog.csdn.net/qq_27093465/article/details/63683873`

### OpenStack与自己的接口对应的maping文件写法
1. 根据自己接口的事件去对应的OpenStack官网中查询对应的事件，包括路径，请求等，并对应下边的示例填入其中。

	    {
	      "Method": "POST",
	      "ActionName": "RemoveSecurityGroup",
	      "Path": "/servers/{server_id}/action",
	      "Parameters": [
	        {
	          "Name": "server_id",
	          "Position": "Path"
	        }
	      ]
	    }
2. 新建一个json,其内容为OpenStack事件中参数与对应的自己接口参数的整理

	    {
		  "Mappings": [
		    {
		      "Request": [
		        {
		          "JPath": "server_id",
		          "MPath": "ResourceId"
		        },
		        {
		          "JPath": "removeSecurityGroup.name",
		          "MPath": "SecurityGroupName"
		        }
		      ]
		    },
		    {
		      "Response": [
		      ]
		    },
		    {
		      "Error": [
		      ]
		    }
		  ]
		}

### 在APIMetas和Waggle的基础上实现一个接口的写法
#### 以HelloWorld为例
1. 创建一个名字为HelloWorld的maven项目，在src->main->java中创建一个名为com.apimetas.helloworld的包，并在包中创建一个名为main和名为HelloWorldServer的文件
2. main 文件中的内容为创建一个类,文件内容如下

		public class Main {
		    public static void main(String[] args) {
		        String haOther = null;
		        if (args.length >= 1)
		            haOther = args[0];
		        HelloWorldServer server = new HelloWorldServer("./data").setupServer(haOther);
		        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
		                server.stopServer();
		                System.exit(0);
		            }));
		
		        int milliSleep = 200;
		        while (true) {
		            try {
		                Thread.currentThread().sleep(milliSleep);
		            } catch(Exception exp) {
		                ;
		            }
		        }
		    }
		
		}
3. 定义数据结构，例如helloWord数据中，key只为HelloWorldID,则新建一个类，如下：

		package com.apimetas.metric;
		import com.apiherd.tenantdb.TreedJson;
		public class Host extends TreedJson {
		    public HelloWorld(TreedJson father) {
		        super(father, "HelloWorldID");
		    }
		}

4. 在HelloWorldServer文件中，写数据结构，owner为最父级，每个括号里面的值为新定义的父级数据结构

		private static TenantUser owner = new TenantUser();
		private static ComputeLevel computeLevel = new ComputeLevel(owner);
		private static ResourceType resourceType = new ResourceType(computeLevel);
		private static Region region = new Region(resourceType);	
5. 如果没有特殊的处理逻辑，可以直接注册接口，包含了，put,get,delete等，也可以在registerAPIs中重新写接口内容。

		counterThreads.registerAPI("PutStream", stream,new APIable() {
            public JSONObject invokeAPI(RawsRequest request, WriteableChannel channel) {
                APIRequest api = (APIRequest) request;
                ...写接口逻辑
                return tree.putJson(stream, api);
            }
        });
            

6. 测试用例的方法，在pom.xml中引入hello world的依赖，并使用helloworldServer。
7. 在main文件中引入测试文件，如下代码

		if (args[0].startsWith("t")) {
		    OpernFactory.parseOperns(loader, "OpernTest.yaml");
		    LogParser.parseLogs(OpernFactory.runOperns(client));
		}

8. 在新建一个OpernTest.yaml文件，并在文件写测试的代码，其中Request中的name中为测试数据（即模拟前端传入的参数），Actions下的name 为接口名称。将Actions 中的Assertion里面的数据去与Meta中的name中的数据对应。
		
		HelloWorld:
		  Meta:
		    Name: APIMetas.json
		  Request:
		    Name: '{"owner":100}'
		  Actions:
		  - Name: GetHelloWorld
		    Assertion:
		      Method: Equal
		      Key: helloworld112
		      Value: Hello Woldw
9. APIMetas.json中的数据如下，只需要改变APIMetas.json即可，其他不是必要改

		{
		  "ActionName":"{PlaceHolder}",
		  "UserId":10000000000,
		  "ServiceName":"HelloWorld",
		  "ServiceVersion":"20190329",
		  "ServiceStage":"Development",
		  "RequestId":"13152fae-d25a-4d78-b318-74397eb08184"
		}	
		
### JSONObject的用法

##### 参考文章： `https://blog.csdn.net/zty1317313805/article/details/80097511`

1. 直接构建即直接实例化一个JSONObject对象，而后调用其put()方法，将数据写入。put()方法的第一个参数为key值，必须为String类型，第二个参数为value，可以为boolean、double、int、long、Object、Map以及Collection等

		JSONObject obj = new JSONObject();
		obj.put(key, value);		
		
2. 解析字符串

* 基本类型的解析直接调用JSONObject对象的getXxx(key)方法，如果获取字符串则getString(key)，布尔值则getBoolean(key)，以此类推。

* 数组的解析稍微麻烦一点，需要通过JSONObject对象的getJSONArray(key)方法获取到一个JSONArray对象，再调用JSONArray对象的get(i)方法获取数组元素，i为索引值。


### 获取JSONArray中的每一条数据

	for (int i = 0; i < json.length(); i++) {
		JSONObject jsonObj = json.getJSONObject(i);//获取数组中的每行
		String id = jsonObj.getString("ID");获取第i行中的属性值
		String name = jsonObj.getString("name");
	}
	
1. 定义一个list集合，并向其中添加数据

		 List numList = new ArrayList();
		 numList.add(anajson);	
	