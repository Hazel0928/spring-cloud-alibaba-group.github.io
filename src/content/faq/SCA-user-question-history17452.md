---
id: "question-history-17452"
title: "给我详细代码和配置： spring ai alibaba 怎么调用其他模型？"
date: "2024-11-08"
category: "expertConsultation"
description: "要调用其他模型，比如OpenAI或本地部署的Ollama框架下的模型，您可以根据`spring ai alibaba`提供的灵活性来配置。以下是针对两种情况（使用OpenAI接口和接入本地部署的Ollama模型）的具体步骤。### 一、通过Spring AI Alibaba调用OpenAI接口**原"
tags: ["Spring","AI","Alibaba","调用","模型","代码","配置"]
keywords: ["Spring","AI","Alibaba","调用","模型","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么调用其他模型？  

---------------
## 专家官方解答 ：

要调用其他模型，比如OpenAI或本地部署的Ollama框架下的模型，您可以根据`spring ai alibaba`提供的灵活性来配置。以下是针对两种情况（使用OpenAI接口和接入本地部署的Ollama模型）的具体步骤。

### 一、通过Spring AI Alibaba调用OpenAI接口

**原因分析：**
基于给出的知识点，我们知道Spring AI体系旨在提供一种方式以支持多种不同的AI模型，包括但不限于阿里云内的模型如通义千问等。同样地，它也支持外部的服务如OpenAI，这使得开发者能够轻松切换底层AI服务提供商而不需大幅修改代码。

**具体步骤：**

1. **引入必要的依赖**  
   在您的`pom.xml`文件中添加如下依赖，以便项目能够识别并使用OpenAI相关功能。
   ```xml
   <dependency>
     <groupId>org.springframework.ai</groupId>
     <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
     <version>请替换为最新版本号</version>
   </dependency>
   ```

2. **配置application.yaml**  
   接下来，在您的`application.yaml`或`application.properties`中设置OpenAI相关的参数，例如API密钥和基础URL。
   ```yaml
   spring:
     ai:
       openai:
         api-key: 您的OpenAI API Key
         base-url: https://api.openai.com/v1 # 或者您可能使用的代理地址
       dashscope: # 关闭默认的DashScope配置
         chat:
           enabled: false
         embedding:
           enabled: false
         image:
           enabled: false
   ```

3. **编写业务逻辑代码**  
   利用`ChatClient`对象与OpenAI进行交互。这部分内容可以根据具体的业务需求来定制化实现。

**解释说明：**
通过上述步骤，我们实际上是将应用程序从默认的阿里云模型转向了OpenAI提供的服务。这样做不仅增加了应用对不同AI技术的支持范围，同时也体现了Spring AI设计上的高度可扩展性。

---

### 二、接入本地部署的Ollama模型

**原因分析：**
对于那些希望利用自定义训练或者特定优化过的模型的场景来说，直接在本地环境中运行这些模型并通过网络服务访问它们是一种有效的方法。Spring AI提供了相应的机制让我们可以无缝集成这样的本地部署方案。

**具体步骤：**

1. **引入Ollama Starter依赖**  
   首先，在项目的构建文件中加入Ollama的启动器依赖。
   ```xml
   <dependency>
     <groupId>org.springframework.ai</groupId>
     <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
     <version>请替换为最新版本号</version>
   </dependency>
   ```

2. **调整配置项**  
   修改`application.yml`文件，指向您的本地Ollama服务，并关闭不必要的DashScope组件。
   ```yaml
   spring:
     ai:
       ollama:
         base-url: http://localhost:9999 # Ollama服务的实际地址
         chat:
           options:
             model: gemma-stock-1:latest # 使用的具体模型版本
       dashscope:
         chat:
           enabled: false
         embedding:
           enabled: false
         image:
           enabled: false
   ```

3. **开发API接口**  
   类似于前一个例子，这里也需要创建相应的控制器类来处理来自客户端的请求，并且通过已注入的`ChatClient`实例与本地的Ollama服务通信。

**解释说明：**
这种方式非常适合于那些已经拥有自己训练好的模型并且希望通过微服务架构暴露给前端应用的情况。通过简单的配置更改即可让现有的Spring Boot应用快速适配新的后端AI服务，极大地提高了系统的灵活性和可维护性。

综上所述，无论您是想连接到云端的OpenAI还是选择使用本地托管的Ollama模型，Spring AI Alibaba都提供了简便易行的方法来实现这一目标。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 是否支持其他模型比如open ai？ 
 
 * 专家经验：spring ai alibaba 文本生成使用说明 
 
 * 专家经验：spring ai alibaba 图像生成 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17466)给我们反馈。
