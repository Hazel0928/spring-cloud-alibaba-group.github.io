---
id: "question-history-17446"
title: "给我详细代码和配置： spring ai alibaba 怎么识别图片中的文字？"
date: "2024-11-08"
category: "expertConsultation"
description: "根据提供的我了解的信息，要使用Spring AI Alibaba来识别图片中的文字，你需要遵循以下步骤进行配置和编码。以下是详细的步骤说明：### 前置准备：1. 确保你的JDK版本在JDK 17或以上。2. 确保你的Spring Boot版本在3.3.x或以上。3. 在阿里云上申请通义千问的API"
tags: ["图片识别","文字识别","Spring","AI","Alibaba","代码","配置"]
keywords: ["图片识别","文字识别","Spring","AI","Alibaba","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么识别图片中的文字？  

---------------
## 专家官方解答 ：

根据提供的我了解的信息，要使用Spring AI Alibaba来识别图片中的文字，你需要遵循以下步骤进行配置和编码。以下是详细的步骤说明：

### 前置准备：
1. 确保你的JDK版本在JDK 17或以上。
2. 确保你的Spring Boot版本在3.3.x或以上。
3. 在阿里云上申请通义千问的API Key。

### 开通资源并获取API Key：
- 打开[阿里云百炼页面](https://www.aliyun.com/product/bailian) -> 登录您的阿里云账号 -> 选择开通“百炼大模型推理”服务。
- 开通成功后，在控制台右上角点击用户图标 -> API-KEY -> 创建新的API-KEY。请记录这个Key，后面需要配置。

### 配置环境变量
```bash
export AI_DASHSCOPE_API_KEY=你的API-KEY
```

### 开通通义万象图像生成模型：
- 打开百炼控制台 [https://bailian.console.aliyun.com/]
- 从左侧列表栏中选择“模型广场”，然后找到“图像生成”部分，选择通义万象模型对应的“API调用”按钮。
- 记住该模型的英文名称：`qwen-vl-max-latest`，这将是后续配置时指定的模型名。

### Maven仓库与依赖设置
添加Spring仓库到pom.xml文件中，并确保包含必要的依赖项：
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

<dependencies>
    <dependency>
        <groupId>com.alibaba.cloud.ai</groupId>
        <artifactId>spring-ai-alibaba-starter</artifactId>
        <version>1.0.0-M3.1</version>
    </dependency>
    <!-- 其他依赖 -->
</dependencies>
```

### 编写代码
创建一个Controller类，通过注入`ChatModel`实例实现图片识别功能。
```java
@RestController
@RequestMapping("/ai")
public class ImageRecognitionController {
    private final ChatModel chatModel;
    @Value("classpath:yourImage.png") // 替换为你的图片路径
    private Resource imageResource;

    public ImageRecognitionController(ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @GetMapping("/recognizeText")
    public Flux<String> recognizeText() throws Exception {
        List<Media> mediaList = List.of(new Media(MimeTypeUtils.IMAGE_PNG, imageResource));
        UserMessage message = new UserMessage("这些是什么？", mediaList);
        message.getMetadata().put(DashScopeChatModel.MESSAGE_FORMAT, MessageFormat.IMAGE);

        Flux<ChatResponse> responseFlux = chatModel.stream(
                new Prompt(message,
                        DashScopeChatOptions.builder()
                                .withModel("qwen-vl-max-latest")
                                .build()));

        return responseFlux.map(resp -> resp.getResult().getOutput().getContent());
    }
}
```
将待识别的文字图片放置于项目的`src/main/resources`目录下，比如命名为`yourImage.png`。

### 测试
启动应用后，访问 `http://localhost:8080/ai/recognizeText` 即可得到图片中的文字描述。

此方案基于Spring AI Alibaba集成阿里云通义万象模型来实现图片内容的文字描述功能。具体操作包括了前置条件检查、服务开通及API密钥管理、Maven项目配置以及实际的应用逻辑编写等步骤。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 图像识别 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17460)给我们反馈。
