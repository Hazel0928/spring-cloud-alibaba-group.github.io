---
id: "question-history-17445"
title: "给我详细代码和配置： spring ai alibaba 怎么做文生图"
date: "2024-11-08"
category: "expertConsultation"
description: "根据我了解的信息中的信息，Spring AI Alibaba 可以帮助开发者轻松地将阿里云的通义万象图像生成模型集成到自己的Spring Boot项目中。以下是实现文生图功能的具体步骤：### 前置要求：1. 确保您的JDK版本在 JDK 17 或更高。2. 您的Spring Boot版本需为 3."
tags: ["spring","ai","alibaba","文生图","代码","配置","实现"]
keywords: ["spring","ai","alibaba","文生图","代码","配置","实现"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么做文生图  

---------------
## 专家官方解答 ：

根据我了解的信息中的信息，Spring AI Alibaba 可以帮助开发者轻松地将阿里云的通义万象图像生成模型集成到自己的Spring Boot项目中。以下是实现文生图功能的具体步骤：

### 前置要求：
1. 确保您的JDK版本在 JDK 17 或更高。
2. 您的Spring Boot版本需为 3.3.x 或更高。

### 开通与配置资源
1. **开通服务**：访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)并登录您的阿里云账号，选择开通“百炼大模型推理”服务。等待服务开通后，按照指示创建一个新的API-KEY，并记下这个key值，后面会用到。

2. **配置环境变量**：使用上一步获取到的API-KEY来设置环境变量 `AI_DASHSCOPE_API_KEY`。
   ```bash
   export AI_DASHSCOPE_API_KEY=你的API-KEY
   ```

3. **启用通义万象模型**：前往[百炼控制台](https://bailian.console.aliyun.com/)，找到左侧菜单中的模型广场，在其中定位到通义万象图像生成模型，并记录该模型的英文名称（例如 `wanx-v1`），以便稍后在代码中引用。

### Maven依赖项配置
请确保您的pom.xml文件包含了如下仓库和依赖项定义，以便引入必要的库支持。

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
    <!-- 添加对 Spring AI Alibaba 的依赖 -->
    <dependency>
        <groupId>com.alibaba.cloud.ai</groupId>
        <artifactId>spring-ai-alibaba-starter</artifactId>
        <version>1.0.0-M2.1</version>
    </dependency>
    <!-- Spring Boot parent project -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.4</version>
        <relativePath/>
    </parent>
    ...其他依赖...
</dependencies>
```

### 创建控制器类

接下来，请在您的项目中添加一个控制器类，通过它来调用通义万象进行图像生成。这里是一个简单的例子：

```java
@RestController
@RequestMapping("/ai")
public class ImageModelController {

    private final ImageModel imageModel;

    @Autowired
    public ImageModelController(ImageModel imageModel) {
        this.imageModel = imageModel;
    }

    @GetMapping("/image")
    public String generateImage(@RequestParam("input") String input) {
        // 构建请求选项
        ImageOptions options = ImageOptionsBuilder.builder()
                .withWidth(1024)
                .withHeight(1024)
                .withModel("wanx-v1")  // 这里填写从阿里云获取的实际模型名称
                .withN(1)
                .build();
        
        // 发送请求
        ImageResponse response = imageModel.call(new ImagePrompt(input, options));
        
        // 获取并返回图片URL
        return response.getResults().get(0).getOutput().getUrl();
    }
}
```

最后不要忘记在application.properties或者application.yml中配置API Key:

```properties
spring.ai.dashscope.api-key=${AI_DASHSCOPE_API_KEY}
```

这样就完成了基本配置。当您启动应用程序并通过`http://localhost:8080/ai/image?input=描述文本`访问时，应该能够收到一张基于输入文本生成的图片链接了。

以上步骤遵循了我了解的信息给出的指导方针，并结合实际情况提供了详细的实现指南。希望这对您有所帮助！


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：spring ai alibaba 图像生成 
 
 * 专家经验：sca ai (spring ai alibaba）介绍 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17459)给我们反馈。
