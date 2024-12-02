---
id: "question-history-17427"
title: "接一个模型，就要写一个starter，这个是spring ai推荐的方式么"
date: "2024-10-30"
category: "expertConsultation"
description: "根据我了解的信息中的内容，Spring AI 的设计目标是通过提供一套良好的抽象层来简化与不同AI提供商（例如OpenAI, Azure, 阿里云等）的接口对接过程。这意味着开发者不需要为每个新的模型单独编写一个starter，而是利用Spring AI已经提供的统一接口来实现快速集成和切换不同的A"
tags: ["模型","starter","spring","ai","推荐","方式"]
keywords: ["模型","starter","spring","ai","推荐","方式"]
---

## 用户问题 ： 
 接一个模型，就要写一个starter，这个是spring ai推荐的方式么  

---------------
## 专家官方解答 ：

根据我了解的信息中的内容，Spring AI 的设计目标是通过提供一套良好的抽象层来简化与不同AI提供商（例如OpenAI, Azure, 阿里云等）的接口对接过程。这意味着开发者不需要为每个新的模型单独编写一个starter，而是利用Spring AI已经提供的统一接口来实现快速集成和切换不同的AI服务。因此，直接为每一个新接入的模型编写一个独立的starter并不是Spring AI推荐的做法。

Spring AI强调的是其框架内部对于多种AI服务商的支持以及灵活配置的能力，让开发者能够更专注于业务逻辑的实现而不是基础架构的构建。比如，在使用Spring AI Alibaba时，仅需添加相应的依赖并按照文档配置好阿里云的服务参数（如API Key），便可以直接调用通义千问这样的大模型进行文本生成、对话等功能开发了，而无需为特定模型额外创建starter项目。

所以，对于您的问题“接一个模型，就要写一个starter，这个是spring ai推荐的方式么？”的答案是否定的。Spring AI鼓励采用更加高效且通用的方法来整合各种AI能力到应用程序中。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：sca ai (spring ai alibaba）介绍 
 
 * 专家经验：spring ai alibaba 文本生成使用说明 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17434)给我们反馈。
