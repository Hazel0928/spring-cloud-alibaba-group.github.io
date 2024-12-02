---
id: "question-history-17450"
title: "给我详细代码和配置： spring ai alibaba 怎么调用通义千问？"
date: "2024-11-08"
category: "expertConsultation"
description: "基于提供的我了解的信息，要使用Spring AI Alibaba调用通义千问，您需要按照以下步骤进行配置和编码：### 前置条件1. **确保您的JDK版本为17或更高**。2. **您的Spring Boot版本至少为3.3.x**。### 步骤 1: 获取API Key- 访问[阿里云百炼页面]"
tags: ["Spring","AI","Alibaba","通义千问","调用","代码","配置"]
keywords: ["Spring","AI","Alibaba","通义千问","调用","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么调用通义千问？  

---------------
## 专家官方解答 ：

基于提供的我了解的信息，要使用Spring AI Alibaba调用通义千问，您需要按照以下步骤进行配置和编码：

### 前置条件
1. **确保您的JDK版本为17或更高**。
2. **您的Spring Boot版本至少为3.3.x**。

### 步骤 1: 获取API Key
- 访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)并登录您的账号。
- 开通“百炼大模型推理”服务，并创建一个新的API KEY。请记录这个key以备后用。

### 步骤 2: 配置环境变量
在项目根目录下或适当位置设置环境变量`AI_DASHSCOPE_API_KEY`来存储你的API key：
```bash
export AI_DASHSCOPE_API_KEY=${REPLACE-WITH-VALID-API-KEY}
```

或者直接在`application.properties`中添加如下行（推荐）:
```properties
spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
```

### 步骤 3: 添加Maven仓库与依赖
在`pom.xml`文件中加入必要的仓库地址及`spring-ai-alibaba-starter`依赖项。这包括Spring自己的仓库以及snapshot仓库。

```xml
<repositories>
    <repository>
      <id>sonatype-snapshots</id>
      <url>https://oss.sonatype.org/content/repositories/snapshots</url>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
    <repository>
      <id>spring-milestones</id>
      <name>Spring Milestones</name>
      <url>https://repo.spring.io/milestone</url>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
    <repository>
      <id>spring-snapshots</id>
      <name>Spring Snapshots</name>
      <url>https://repo.spring.io/snapshot</url>
      <releases>
        <enabled>false</enabled>
      </releases>
    </repository>
</repositories>

<dependencies>
  <!-- Spring Boot Parent Project -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.3.4</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
  
  <!-- Alibaba Cloud AI Starter -->
  <dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter</artifactId>
    <version>1.0.0-M3.1</version>
  </dependency>
  <!-- Other dependencies... -->
</dependencies>
```

### 步骤 4: 编写Controller
接下来，你需要创建一个REST控制器类来处理请求，并通过注入`ChatClient`实例实现与通义千问的交互。

```java
@RestController
@RequestMapping("/ai")
public class ChatController {

  private final ChatClient chatClient;

  public ChatController(ChatClient.Builder builder) {
    this.chatClient = builder.build();
  }

  @GetMapping("/chat")
  public String chat(@RequestParam String input) {
    return this.chatClient.prompt()
                          .user(input)
                          .call()
                          .content();
  }
}
```

### 解释
以上步骤完成了从准备到实际应用的所有过程。首先设置了项目的运行环境和所需的库支持；接着获取了用于身份验证的API密钥；最后编写了一个简单的Spring MVC Controller，它利用`ChatClient`向通义千问发送消息并接收响应。这样的架构不仅简化了与大型语言模型交互的过程，同时也保持了代码的简洁性和可维护性。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 文本生成使用说明 
 
 * 专家经验：Spring ai alibaba 使用RAG能力 
 
 * 专家经验：spring ai alibaba 是否支持其他模型比如open ai？ 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17464)给我们反馈。
