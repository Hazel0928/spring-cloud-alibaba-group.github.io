---
id: "question-history-17448"
title: "给我详细代码和配置： spring ai alibaba 怎么识别车牌号？"
date: "2024-11-08"
category: "expertConsultation"
description: "根据您的需求，要使用Spring AI Alibaba来识别车牌号，我们可以基于提供的知识进行相应的配置和代码实现。以下是详细的步骤：### 前置准备1. 确保JDK版本在JDK 17（含）以上。2. 使用的Spring Boot版本为3.3.x及以上。3. 在阿里云申请通义千问的API Key： "
tags: ["Spring","AI","Alibaba","车牌识别","代码","配置","教程"]
keywords: ["Spring","AI","Alibaba","车牌识别","代码","配置","教程"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么识别车牌号？  

---------------
## 专家官方解答 ：

根据您的需求，要使用Spring AI Alibaba来识别车牌号，我们可以基于提供的知识进行相应的配置和代码实现。以下是详细的步骤：

### 前置准备
1. 确保JDK版本在JDK 17（含）以上。
2. 使用的Spring Boot版本为3.3.x及以上。
3. 在阿里云申请通义千问的API Key：
   - 访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)，登录您的阿里云账号。
   - 开通“百炼大模型推理”服务，并等待开通成功的短信通知。
   - 成功开通后，在阿里云百炼页面右上角选择小人图标 -> API-KEY -> 创建新的API-KEY并记录该Key。

4. 配置百炼 API KEY:
   ```bash
   export AI_DASHSCOPE_API_KEY=${REPLACE-WITH-VALID-API-KEY}
   ```

5. 开通通义万象图像生成模型：
   - 打开[百炼控制台](https://bailian.console.aliyun.com/)。
   - 从左侧列表栏选择模型广场，找到图像生成类别下的通义万象模型，并点击其对应的“API调用”按钮。
   - 复制该模型的英文名称`qwen-vl-max-latest`，这将是后续配置中需要指定的模型名字。

### Maven仓库与依赖添加
将以下内容加入到项目的pom.xml文件中以添加所需的仓库及依赖项：
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
  <dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter</artifactId>
    <version>1.0.0-M3.1</version>
  </dependency>
  <!-- 其他项目所需依赖 -->
</dependencies>
```

同时确保已设置正确的Spring Boot父级依赖版本。

### 应用属性设置
在application.properties文件中添加如下行以配置API Key:
```properties
spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
```

### 编写控制器代码
接下来是创建一个Controller用于处理车牌号码的图片上传与识别请求。假设您已经准备好了一张包含车牌号码的图片，并将其放置于`src/main/resources/`目录下命名为`carplate.png`。

```java
@RestController
@RequestMapping("/ai")
public class LicensePlateRecognitionController {

    private final ChatModel chatModel;
    
    @Value("classpath:carplate.png")
    private Resource imageResource;

    public LicensePlateRecognitionController(ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    private static final String DEFAULT_MODEL = "qwen-vl-max-latest";
    private static final String LICENSE_PLATE_PROMPT = "请告诉我这张图片中的车牌号码是什么？";

    @GetMapping("/recognizeLicensePlate")
    public Flux<String> recognizeLicensePlate(@RequestParam(value = "prompt", required = false, defaultValue = LICENSE_PLATE_PROMPT) String prompt,
                                              HttpServletResponse response) throws Exception {
        response.setCharacterEncoding("UTF-8");

        List<Media> mediaList = Collections.singletonList(
                new Media(MimeTypeUtils.IMAGE_PNG, imageResource));
        
        UserMessage message = new UserMessage(prompt, mediaList);
        message.getMetadata().put(DashScopeChatModel.MESSAGE_FORMAT, MessageFormat.IMAGE);

        DashScopeChatOptions options = DashScopeChatOptions.builder()
            .withModel(DEFAULT_MODEL)
            .withMultiModel(true)
            .build();
        
        return chatModel.stream(new Prompt(message, options))
                        .map(resp -> resp.getResult().getOutput().getContent());
    }
}
```

通过访问 `http://localhost:8080/ai/recognizeLicensePlate` 即可调用此接口让模型尝试识别图片中的车牌号码。

请注意，这里的例子仅作为演示用途。实际应用时可能还需要考虑更多因素如错误处理、安全性等。此外，由于车牌识别的具体效果还受到图片质量等多种因素影响，请确保输入给模型的图片清晰度足够高。


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
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17462)给我们反馈。
