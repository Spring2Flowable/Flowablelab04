

------

***环境：***

**[tomcat8]()**

**[jdk1.8+]()**

[**flowable6.4.0**]()

------



## 一、 部署

　[参考官网](https://www.flowable.org/docs/userguide/index.html#getting.started.rest)

-   [下载Flowable6.4.0软件包](https://github.com/flowable/flowable-engine/releases/download/flowable-6.4.0/tomcat-flowable-6.4.0.zip)

- 解压
- 将flowable-rest.war放在tomcat的webapp目录下

## 二、启动tomcat

```
linux系统：./startup.sh 
win系统：./startup.bat 
```



## 三、测试rest api

### 3.1 检查应用是否正常

```
curl --user rest-admin:test http://localhost:8080/flowable-rest/service/management/engine
```

### 3.2 	部署流程定义

- 上传流程文件

```
curl --user rest-admin:test -F "file=@holiday-request.bpmn20.xml" http://localhost:8080/flowable-rest/service/repository/deployments
```

- 流程定义

```
curl --user rest-admin:test http://localhost:8080/flowable-rest/service/repository/process-definitions
```

### 3.3 开始流程实例

```
curl --user rest-admin:test -H "Content-Type: application/json" -X POST -d '{ "processDefinitionKey":"holidayRequest", "variables": [ { "name":"employee", "value": "John Doe" }, { "name":"nrOfHolidays", "value": 7 }]}' http://localhost:8080/flowable-rest/service/runtime/process-instances
```

### 3.4 查询流程信息

-  查询managers组下的流程

```
curl --user rest-admin:test -H "Content-Type: application/json" -X POST -d '{ "candidateGroup" : "managers" }' http://localhost:8080/flowable-rest/service/query/tasks
```

### 3.5 完成任务

```
curl --user rest-admin:test -H "Content-Type: application/json" -X POST -d '{ "action" : "complete", "variables" : [ { "name" : "approved", "value" : true} ]  }' http://localhost:8080/flowable-rest/service/runtime/tasks/25
```



注意：

如果出现以下错误：

```
{"message":"Internal server error","exception":"couldn't instantiate class org.flowable.CallExternalSystemDelegate"}
```

缺少CallExternalSystemDelegate类，[参考Flowablelab03](https://github.com/Spring2Flowable/Flowablelab03/blob/master/flowable-lab03/src/main/java/com/flowable/demo/Delegate/CallExternalSystemDelegate.java)，将此编译后的calss文件放置在 **flowable-res**t的***WEB-INF/lib***下面即可。

