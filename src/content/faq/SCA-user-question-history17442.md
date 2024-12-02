---
id: "question-history-17442"
title: "给我详细代码和配置： spring ai alibaba 怎么用做function ca"
date: "2024-11-08"
category: "expertConsultation"
description: "根据您提供的我了解的信息，要实现`spring ai alibaba`中的函数调用（Function Calling），我们需要完成几个关键步骤：定义Java函数、注册该函数至Spring配置中、以及通过适当的接口将Prompt和函数一同提供给服务。接下来，我将基于这些信息为您提供详细的执行步骤。#"
tags: ["Spring","AI","Alibaba","Function","Call","配置","代码"]
keywords: ["Spring","AI","Alibaba","Function","Call","配置","代码"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么用做function call函数调用  

---------------
## 专家官方解答 ：

根据您提供的我了解的信息，要实现`spring ai alibaba`中的函数调用（Function Calling），我们需要完成几个关键步骤：定义Java函数、注册该函数至Spring配置中、以及通过适当的接口将Prompt和函数一同提供给服务。接下来，我将基于这些信息为您提供详细的执行步骤。

### 第一步：定义一个函数
首先，您需要创建一个实现了`java.util.function.Function`接口的类，这将作为我们希望让大模型调用的功能。下面的例子展示了如何定义这样一个查询消息状态的服务函数：

```java
import java.util.function.Function;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonPropertyDescription;

public class MessageStatusService implements Function<MessageStatusService.MessageStatusRequest, String> {
    @Override
    public String apply(MessageStatusService.MessageStatusRequest t) {
        return t.getMessageId() + " 的状态是：消息正常";
    }

    public static class MessageStatusRequest {
        @JsonProperty(required = true, value = "消息id")
        @JsonPropertyDescription("消息id, 比如123123***")
        private String messageId;

        // 构造方法、getter和setter省略...
    }
}
```

这里的关键点在于使用了`@JsonProperty`和`@JsonPropertyDescription`注解来帮助大模型理解函数的输入输出参数及其意义。

### 第二步：注册函数
接着，在您的Spring Boot应用中注册上面定义好的函数，以便于后续被`spring ai alibaba`框架识别并利用。这通常是在某个配置类里通过`@Bean`注解来完成的：

```java
@Configuration
public class AppConfig {
    @Bean
    @Description("查询指定消息ID的状态")
    public Function<MessageStatusService.MessageStatusRequest, String> messageStatusFunction() {
        return new MessageStatusService();
    }
}
```

这里的`@Description`用来向大模型解释这个函数的主要用途。

### 第三步：通过API接口结合Prompt与函数
最后一步是编写控制器层代码，使得用户可以通过HTTP请求触发特定的Prompt，并且当大模型认为有必要时能够调用我们之前定义并注册过的函数。这里推荐采用`PromptTemplate`的方式进行处理：

```java
@RestController
@RequestMapping("/ai")
@CrossOrigin(origins = "*")
public class ChatController {
    @GetMapping("/status")
    public Flux<String> checkMessageStatus(@RequestParam String id) {
        PromptTemplate promptTemplate = new PromptTemplate("我想知道消息id: {id}");
        DashscopeChatOptionsBuilder opsBuilder = DashscopeChatOptions.builder().withFunction("messageStatusFunction");
        Map<String, Object> map = Map.of("id", id);
        Prompt promp = promptTemplate.create(map, opsBuilder.build());
        return chatClient.prompt(promp).stream().content();  // 假设chatClient已经正确初始化
    }
}
```

这段代码定义了一个RESTful API端点，它接收一个消息ID作为参数，然后构造相应的Prompt并通过已配置好的`chatClient`发送出去。

### 解释
- 我们通过定义具体功能的Java函数，并为其添加合适的注解以提高可读性和易理解性。
- 函数注册到Spring上下文中后，便可以被`spring ai alibaba`框架识别并供大模型选择性地调用。
- 最终，通过构建包含必要信息的Prompt模板及对应的选项设置，我们能够灵活地控制何时以及如何触发函数调用，从而增强与AI模型交互的能力。

以上就是基于`spring ai alibaba`实现函数调用的具体做法。希望这对您有所帮助！如果有更多问题或需要进一步的帮助，请随时告诉我。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba  Function Calling 
 
 * 专家经验：sca ai 的prompt用法说明 
 
 * 专家经验：Spring ai alibaba 使用RAG能力 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17456)给我们反馈。
