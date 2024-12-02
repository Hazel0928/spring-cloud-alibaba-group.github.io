---
id: "question-history-17449"
title: "给我详细代码和配置： spring ai alibaba 怎么调用大模型？"
date: "2024-11-08"
category: "expertConsultation"
description: "基于您提出的需求“spring ai alibaba 怎么调用大模型”，根据我了解的信息中的信息，我们可以总结出以下步骤来完成这一目标。这些步骤包括环境配置、依赖项添加、API Key获取与配置，以及最后的代码实现。### 步骤 1: 确认开发环境- **确保JDK版本为17或更高**。- **确保"
tags: ["spring","ai","alibaba","调用","大模型","代码","配置"]
keywords: ["spring","ai","alibaba","调用","大模型","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么调用大模型？  

---------------
## 专家官方解答 ：

基于您提出的需求“spring ai alibaba 怎么调用大模型”，根据我了解的信息中的信息，我们可以总结出以下步骤来完成这一目标。这些步骤包括环境配置、依赖项添加、API Key获取与配置，以及最后的代码实现。

### 步骤 1: 确认开发环境
- **确保JDK版本为17或更高**。
- **确保Spring Boot版本为3.3.x或更高**。

### 步骤 2: 获取阿里云通义千问API Key
- 访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)。
- 登录您的阿里云账号。
- 开通“百炼大模型推理”服务。
- 成功开通后，创建一个新的API Key，并妥善保管。

### 步骤 3: 配置API Key
- 将API Key设置到系统环境变量中：
  ```bash
  export AI_DASHSCOPE_API_KEY=${REPLACE-WITH-VALID-API-KEY}
  ```
- 或者直接在`application.properties`文件中指定API Key:
  ```properties
  spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
  ```

### 步骤 4: 添加仓库和依赖
- 在项目的pom.xml文件中加入Spring及阿里巴巴AI相关仓库：
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
  ```
- 添加必要的依赖项以启用Spring AI Alibaba支持：
  ```xml
  <dependencies>
    <dependency>
      <groupId>com.alibaba.cloud.ai</groupId>
      <artifactId>spring-ai-alibaba-starter</artifactId>
      <version>1.0.0-M3.1</version>
    </dependency>
  </dependencies>
  ```

### 步骤 5: 编写控制器和服务类
- 创建一个Controller类，其中包含处理请求的方法。通过构造函数注入`ChatClient`实例，以便能够与AI进行交互。
  ```java
  @RestController
  public class ChatController {
  
    private final ChatClient chatClient;
    
    public ChatController(ChatClient.Builder builder) {
      this.chatClient = builder.build();
    }
  
    @GetMapping("/chat")
    public String chat(@RequestParam("input") String input) {
      return this.chatClient.prompt()
        .user(input)
        .call()
        .content();
    }
  }
  ```

以上就是利用Spring AI Alibaba调用通义千问大模型的基本过程。该流程涵盖了从前期准备到实际编码的所有必要步骤。如果需要进一步定制化配置（如更改使用的模型等），可以参考官方文档提供的更多选项进行调整。希望这能帮助你成功地集成AI能力到你的项目中！


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 文本生成使用说明 
 
 * 专家经验：spring ai alibaba 图像生成 
 
 * 专家经验：spring ai alibaba 是否支持其他模型比如open ai？ 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17463)给我们反馈。
