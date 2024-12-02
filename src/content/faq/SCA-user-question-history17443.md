---
id: "question-history-17443"
title: "给我详细代码和配置： spring ai alibaba 怎么把AI的内容转化为java"
date: "2024-11-08"
category: "expertConsultation"
description: "根据我了解的信息中的内容，您希望将AI生成的内容转化为JavaBean。这里提供一种方法，基于Spring AI Alibaba实现这一目标。该过程包括设置环境、配置API密钥、添加依赖项以及编写结构化输出的代码。接下来，我将按照步骤详细说明如何操作。### 第一步：前置准备- 确保您的JDK版本至"
tags: ["Spring","AI","Alibaba","JavaBean","转化","配置","代码"]
keywords: ["Spring","AI","Alibaba","JavaBean","转化","配置","代码"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么把AI的内容转化为javabean  

---------------
## 专家官方解答 ：

根据我了解的信息中的内容，您希望将AI生成的内容转化为JavaBean。这里提供一种方法，基于Spring AI Alibaba实现这一目标。该过程包括设置环境、配置API密钥、添加依赖项以及编写结构化输出的代码。接下来，我将按照步骤详细说明如何操作。

### 第一步：前置准备
- 确保您的JDK版本至少为17。
- 使用Spring Boot版本3.3.x及以上。
- 在阿里云上申请通义千问的API Key，并记录下来以备后续使用。

### 第二步：配置阿里云API KEY
在终端或命令行中执行以下命令来设置环境变量（请替换`REPLACE-WITH-VALID-API-KEY`为实际获得的API Key）:
```bash
export AI_DASHSCOPE_API_KEY=REPLACE-WITH-VALID-API-KEY
```

同时，在项目的`application.properties`文件里加入：
```properties
spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
```

### 第三步：添加仓库与依赖
由于`spring-ai-alibaba-starter`尚未发布到Maven中央仓库，因此需要先添加Spring自有的仓库：

```xml
<repositories>
    <repository>
        <id>sonatype-snapshots</id>
        <url>https://oss.sonatype.org/content/repositories/snapshots</url>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
        <snapshots><enabled>false</enabled></snapshots>
    </repository>
    <repository>
        <id>spring-snapshots</id>
        <name>Spring Snapshots</name>
        <url>https://repo.spring.io/snapshot</url>
        <releases><enabled>false</enabled></releases>
    </repository>
</repositories>
```

接着，在`pom.xml`中添加`spring-ai-alibaba-starter`依赖：

```xml
<dependencies>
  <dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter</artifactId>
    <version>1.0.0-M3.1</version>
  </dependency>
  <!-- 其他依赖 -->
</dependencies>
```

### 第四步：定义JavaBean
创建一个简单的JavaBean类用于接收从AI模型返回的数据。例如，下面是一个表示演员及其电影列表的简单实体类：

```java
package com.example.demo.entity;

import java.util.List;

public class ActorsFilms {
  private String actor;
  private List<String> movies;

  // Getters and Setters

  @Override
  public String toString() {
    return "ActorsFilms{" + "actor='" + actor + '\'' + ", movies=" + movies + '}';
  }
}
```

### 第五步：编写控制器处理逻辑
创建一个新的控制器来调用AI服务并解析响应结果为Java对象。这涉及到使用`ChatClient`与特定格式的消息交互，并通过`BeanOutputConverter`将字符串形式的结果转换成之前定义好的JavaBean对象。

```java
import com.alibaba.cloud.ai.example.outparser.entity.ActorsFilms;
import reactor.core.publisher.Flux;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.converter.BeanOutputConverter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.ParameterizedTypeReference;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/ai")
public class OutputParserController {

  private final ChatClient chatClient;
  private final ChatModel chatModel;

  @Autowired
  public OutputParserController(ChatClient.Builder builder, ChatModel chatModel) {
    this.chatClient = builder.build();
    this.chatModel = chatModel;
  }

  @GetMapping("/output/stream")
  public List<ActorsFilms> generateStream(@RequestParam(value = "actor", defaultValue = "Jeff Bridges") String actor) {
    var converter = new BeanOutputConverter<>(new ParameterizedTypeReference<List<ActorsFilms>>() {});
    
    Flux<String> flux = this.chatClient.prompt()
      .user(u -> u.text("""
                        Generate the filmography for a random actor.
                        {format}
                      """)
          .param("format", converter.getFormat()))
      .stream()
      .content();

    return converter.convert(String.join("", Objects.requireNonNull(flux.collectList().block())));
  }
}
```

以上步骤描述了如何利用Spring AI Alibaba框架将AI生成的内容转换为JavaBean的过程。首先确保满足软件环境要求，然后正确配置API密钥，并通过指定的Maven仓库和依赖关系引入必要的库。最后，通过定义合适的JavaBean及相应的控制器方法来完成数据解析任务。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 结构化对象输出 
 
 * 专家经验：sca ai (spring ai alibaba）介绍 
 
 * 专家经验：spring ai alibaba 文转语音输出 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17457)给我们反馈。
