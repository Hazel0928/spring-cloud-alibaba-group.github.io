---
id: "question-history-17429"
title: "看了下Spring Ai Alibaba项目代码，主要是针对通义模型做了实现，如果想接入"
date: "2024-10-30"
category: "expertConsultation"
description: "根据我了解的信息中提供的信息，Spring AI Alibaba 的设计目标是提供一种标准化的方式来接入不同的AI模型供应商（如阿里云的通义、OpenAI等），并尽可能减少开发者在切换不同AI服务时所需的工作量。这意味着Spring AI Alibaba本身已经支持了一定范围内的自定义配置来适应非默"
tags: ["Spring","Ai","Alibaba","通义模型","自定义模型","Eas部署","接口实现"]
keywords: ["Spring","Ai","Alibaba","通义模型","自定义模型","Eas部署","接口实现"]
---

## 用户问题 ： 
 看了下Spring Ai Alibaba项目代码，主要是针对通义模型做了实现，如果想接入其他自定义的模型，比如从ALiyun上的Eas部署的模型，是不是还要针对接口再做一次实现  

---------------
## 专家官方解答 ：

根据我了解的信息中提供的信息，Spring AI Alibaba 的设计目标是提供一种标准化的方式来接入不同的AI模型供应商（如阿里云的通义、OpenAI等），并尽可能减少开发者在切换不同AI服务时所需的工作量。这意味着Spring AI Alibaba本身已经支持了一定范围内的自定义配置来适应非默认集成的AI模型。

### 问题分析
对于您提到的情况——想接入通过阿里云EAS部署的自定义模型，确实可能需要进行额外的开发工作，因为目前直接给出的支持文档更多集中在已知的服务商上。但是，基于Spring AI的设计理念“写一次代码，支持所有模型”，理论上应该可以通过适当调整配置或添加少量定制化代码来实现对新模型的支持。

### 建议步骤
虽然没有具体的官方指南说明如何为阿里云EAS上的自定义模型做适配，但从已有信息可以推导出一个大概的操作流程：

1. **引入必要的依赖**：首先确保您的项目包含了最新版本的`spring-ai-alibaba`以及其他可能需要的库。
2. **配置文件调整**：
   - 如果您计划使用类似于上面提到的方法（比如OpenAI或者本地Ollama模型）来接入自定义模型，则需要在`application.properties`或`application.yml`中添加相应配置项。
   - 参考给定的例子，您可以尝试设置类似的属性以指向您的EAS服务地址和认证信息。例如:
     ```yaml
     spring:
       ai:
         eas:  # 自定义配置名
           base-url: https://your-eas-service-url
           api-key: your-api-key
     ```
3. **编写适配器**：如果现有的配置不足以满足需求，那么您可能需要创建一个新的`ModelAdapter`类来处理与EAS服务之间的通信逻辑。这包括但不限于请求构建、响应解析等功能。
4. **启用新的适配器**：在Spring配置中注册您刚刚编写的适配器，并禁用不需要的服务（如果适用的话）。参考之前关于关闭dashscope默认加载的做法。
5. **测试连接**：完成上述步骤后，请务必彻底测试新的集成点，确保它可以正常工作并且能够按预期返回结果。

### 解释
这个过程主要是为了让Spring AI阿里巴巴框架能够识别并通过HTTP或其他协议调用到您的EAS托管的AI模型。通过这种方式，即使是在非标准情况下也能利用Spring AI的灵活性优势，快速整合进新的AI能力而不必完全重写应用程序。请注意，具体实现细节可能会因实际情况而有所不同，建议查阅相关API文档及社区资源获取更详细的指导。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 是否支持其他模型比如open ai？ 
 
 * 专家经验：sca ai (spring ai alibaba）介绍 
 
 * 专家经验：spring ai alibaba支持的向量存储和嵌入方案 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17436)给我们反馈。
