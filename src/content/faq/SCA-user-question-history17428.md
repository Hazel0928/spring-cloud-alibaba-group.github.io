---
id: "question-history-17428"
title: "给我一个自定义接入模型的例子"
date: "2024-10-30"
category: "expertConsultation"
description: "根据我了解的信息中提供的信息，自定义接入模型可以通过配置`spring-ai-ollama-spring-boot-starter`来实现本地部署的模型支持。下面将按照这一逻辑详细说明如何完成自定义模型（这里以Ollama框架为例）的接入。### 原因分析Spring AI Alibaba设计初衷是"
tags: ["自定义","接入","模型","示例","示例例子","示例案例","自定义模型"]
keywords: ["自定义","接入","模型","示例","示例例子","示例案例","自定义模型"]
---

## 用户问题 ： 
 给我一个自定义接入模型的例子  

---------------
## 专家官方解答 ：

根据我了解的信息中提供的信息，自定义接入模型可以通过配置`spring-ai-ollama-spring-boot-starter`来实现本地部署的模型支持。下面将按照这一逻辑详细说明如何完成自定义模型（这里以Ollama框架为例）的接入。

### 原因分析
Spring AI Alibaba设计初衷是让开发者能够通过一套代码适配多种AI模型。为了满足特定场景下使用非默认支持模型的需求（如本例中的自定义或本地部署的Ollama模型），Spring AI提供了灵活的配置选项，允许指定额外的模型来源和服务地址。

### 具体步骤

1. **关闭DashScope默认加载**
   - 在您的`application.properties`或`application.yml`文件里加入如下设置，确保禁用了DashScope的相关功能，以便于开启对Ollama的支持：
     ```yaml
     spring:
       ai:
         dashscope:
           chat:
             enabled: false
           embadding:
             enabled: false
           image:
             enabled: false
     ```

2. **添加Ollama相关配置**
   - 同样在`application.yml`中，为Ollama指定基础URL及所需模型：
     ```yaml
     spring:
       ai:
         ollama:
           base-url: http://localhost:9999  # 您的Ollama服务运行的地址
           chat:
             options:
               model: gemma-stock-1:latest  # 模型名称
     ```

3. **引入Ollama Starter依赖**
   - 确保您的`pom.xml`文件包含了正确的Ollama starter依赖。请注意版本号可能与Spring AI Alibaba有所不同，请参考最新文档确认兼容性。
     ```xml
     <dependency>
       <groupId>org.springframework.ai</groupId>
       <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
       <version>最新spring ai 对应的版本号</version>
     </dependency>
     ```

4. **编写调用逻辑**
   - 使用Spring Boot提供的API来发起请求到已配置好的Ollama服务。这部分可以根据具体需求进行调整，比如创建一个控制器方法处理外部请求，并利用`ChatClient`等组件向Ollama发送查询。

### 解释
以上步骤首先确保了Spring AI Alibaba不会尝试连接其默认支持的服务（如DashScope），然后指定了我们想要使用的本地Ollama服务的具体位置和参数。通过添加相应的Maven依赖并正确配置项目，我们可以无缝地集成任何符合要求的本地模型进入现有的Spring Boot应用中。这不仅增加了灵活性，还使得测试和开发新功能变得更加方便快捷。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 是否支持其他模型比如open ai？ 
 
 * 专家经验：指定单次大模型查询时候的参数 
 
 * 专家经验：spring ai alibaba  Function Calling 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17435)给我们反馈。
