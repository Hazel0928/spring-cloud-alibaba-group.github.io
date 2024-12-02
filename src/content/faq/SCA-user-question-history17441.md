---
id: "question-history-17441"
title: "给我详细代码和配置： spring ai alibaba 怎么用flux stream "
date: "2024-11-08"
category: "expertConsultation"
description: "根据我了解的信息中提供的信息，要使用Spring AI Alibaba并通过Flux Stream流的方式返回结果，你需要遵循以下步骤来配置和编码。这里我将结合第一篇参考内容（关于文字转语音输出）和第三篇参考内容（关于基于通义千问实现聊天交互）中的示例，给出详细的执行步骤。### 前置准备1. 确保"
tags: ["Spring","AI","Alibaba","Flux","Stream","返回结果","配置"]
keywords: ["Spring","AI","Alibaba","Flux","Stream","返回结果","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么用flux stream 流 的方式返回结果  

---------------
## 专家官方解答 ：

根据我了解的信息中提供的信息，要使用Spring AI Alibaba并通过Flux Stream流的方式返回结果，你需要遵循以下步骤来配置和编码。这里我将结合第一篇参考内容（关于文字转语音输出）和第三篇参考内容（关于基于通义千问实现聊天交互）中的示例，给出详细的执行步骤。

### 前置准备
1. 确保你的JDK版本在JDK 17或以上。
2. 确保你的Spring Boot版本为3.3.x及以上。
3. 在阿里云申请API Key，并记录下来用于后续的环境变量配置。

### 配置环境变量
设置环境变量`AI_DASHSCOPE_API_KEY`为你从阿里云获取到的有效API Key：
```bash
export AI_DASHSCOPE_API_KEY=${REPLACE-WITH-VALID-API-KEY}
```

### Maven仓库及依赖添加
在项目的pom.xml文件中加入特定于Spring AI Alibaba所需的Maven仓库以及项目依赖：

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
    <!-- Spring Boot parent project -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.4</version>
        <relativePath/>
    </parent>
    
    <dependency>
        <groupId>com.alibaba.cloud.ai</groupId>
        <artifactId>spring-ai-alibaba-starter</artifactId>
        <version>1.0.0-M3.1</version>
    </dependency>
    <!-- Add other dependencies as needed -->
</dependencies>
```

### Controller类编写
下面是一个简单的控制器示例，该控制器利用了Flux来流式处理并返回来自AI模型的结果。这个例子基于文字转语音功能进行了调整，以适应通过流方式传递数据的需求。

```java
@RestController
@RequestMapping("/ai/audio")
public class AudioStreamController {

    private final SpeechSynthesisModel speechSynthesisModel;

    public AudioStreamController(SpeechSynthesisModel speechSynthesisModel) {
        this.speechSynthesisModel = speechSynthesisModel;
    }

    @GetMapping("/stream")
    public Flux<byte[]> streamTTS(@RequestParam String text) {
        
        // 发起请求给语音合成服务
        Flux<SpeechSynthesisResponse> response = speechSynthesisModel.stream(
                new SpeechSynthesisPrompt(text));

        return response.map(response -> {
            ByteBuffer byteBuffer = response.getResult().getOutput().getAudio();
            return byteBuffer.array();
        });
    }
}
```
在这个控制器中，我们定义了一个`/ai/audio/stream`端点，它接受一个文本参数`text`，然后调用`speechSynthesisModel.stream()`方法产生一个`Flux<SpeechSynthesisResponse>`对象。接着，我们使用`.map()`操作符将每个响应转换为其音频字节数组形式，最终形成了一个可以被客户端订阅的数据流。

### 解释
- **Flux** 是Reactor库提供的一种发布者类型，适用于处理无限数量的异步序列项。在这个场景下，我们使用Flux来代表连续到达的音频数据块。
- **SpeechSynthesisModel** 由`spring-ai-alibaba-starter`自动配置而成，负责与阿里云的文字转语音服务通信。
- **SpeechSynthesisPrompt** 用来封装待转换成语音的原始文本。
- **SpeechSynthesisResponse** 包含了从服务端接收到的一段音频数据及其相关信息。

这样配置后，你可以通过访问类似`http://localhost:8080/ai/audio/stream?text=测试文本`这样的URL来触发语音合成过程，并接收以流的形式传输回来的音频数据。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 文转语音输出 
 
 * 专家经验：spring ai alibaba 结构化对象输出 
 
 * 专家经验：spring ai alibaba 文本生成使用说明 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17455)给我们反馈。
