---
id: "question-history-17444"
title: "给我详细代码和配置： spring ai alibaba 怎么做结构转换"
date: "2024-11-08"
category: "expertConsultation"
description: "要实现通过Spring AI Alibaba进行结构化输出，需要遵循以下步骤。这里将根据提供的我了解的信息内容逐步解析，并给出具体的执行步骤。### 一、原因分析首先，为了能够利用阿里云通义大模型的能力来生成结构化的数据输出（比如Java Bean），必须确保开发环境符合特定要求，并正确配置与使用相"
tags: ["Spring","AI","Alibaba","结构转换","代码","配置"]
keywords: ["Spring","AI","Alibaba","结构转换","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么做结构转换  

---------------
## 专家官方解答 ：

要实现通过Spring AI Alibaba进行结构化输出，需要遵循以下步骤。这里将根据提供的我了解的信息内容逐步解析，并给出具体的执行步骤。

### 一、原因分析
首先，为了能够利用阿里云通义大模型的能力来生成结构化的数据输出（比如Java Bean），必须确保开发环境符合特定要求，并正确配置与使用相应的SDK及API。这包括了JDK版本、Spring Boot版本以及如何获取和设置必要的API密钥等准备工作。

### 二、具体步骤

#### 步骤1：环境准备
- **JDK版本**：确保你的开发环境中的JDK版本不低于17。
- **Spring Boot版本**：项目需基于Spring Boot 3.3.x及以上版本构建。

#### 步骤2：获取API Key
访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)并登录您的账号，按照指引开通“百炼大模型推理”服务后创建一个新的API Key。记住这个Key值，后面会用到它。

#### 步骤3：配置API Key
在命令行或系统环境变量中添加如下设置：
```bash
export AI_DASHSCOPE_API_KEY=your_valid_api_key
```
同时，在`application.properties`文件中加入：
```properties
spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
```

#### 步骤4：修改pom.xml以引入所需依赖
向你的`pom.xml`文件添加Spring仓库信息及依赖项：
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

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.3.4</version>
</parent>
<dependencies>
    <dependency>
        <groupId>com.alibaba.cloud.ai</groupId>
        <artifactId>spring-ai-alibaba-starter</artifactId>
        <version>1.0.0-M3.1</version>
    </dependency>
</dependencies>
```

#### 步骤5：定义结构化输出类
创建一个简单的Java类来表示期望的输出格式，例如：
```java
package com.example;

import java.util.List;

public class ActorsFilms {
    private String actor;
    private List<String> movies; // 其他字段...

    // getters, setters...
}
```

#### 步骤6：编写Controller代码
最后，在控制器中使用`ChatClient`与`BeanOutputConverter`完成从非结构化文本到Java对象的转换：
```java
@RestController
@RequestMapping("/ai")
public class OutputParserController {

    @Autowired
    private ChatClient chatClient;

    @GetMapping("/output/stream")
    public List<ActorsFilms> generateStream(@RequestParam(value = "actor", defaultValue = "Jeff Bridges") String actor) {
        var converter = new BeanOutputConverter<>(new ParameterizedTypeReference<List<ActorsFilms>>() {});
        Flux<String> flux = this.chatClient.prompt()
                .user(u -> u.text("""
                        Generate the filmography for a random actor.
                        {format}
                        """).param("format", converter.getFormat()))
                .stream()
                .content();

        return converter.convert(String.join("", Objects.requireNonNull(flux.collectList().block())));
    }
}
```

### 三、解释
上述步骤指导你如何在Spring Boot应用中集成阿里云通义大模型，以生成特定格式的结构化数据。通过调整输入提示和输出转换器，可以灵活地控制最终得到的数据结构。这样做的好处在于能够直接从自然语言请求转换为应用程序可以直接使用的编程语言对象，极大地简化了处理流程。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 结构化对象输出 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17458)给我们反馈。
