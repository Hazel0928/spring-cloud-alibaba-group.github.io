---
id: "question-history-17453"
title: "给我详细代码和配置： spring ai alibaba 怎么基于自有知识库做rag增强"
date: "2024-11-08"
category: "expertConsultation"
description: "根据提供的我了解的信息，要基于Spring AI Alibaba并使用自有知识库实现RAG（检索增强生成）能力的客服机器人，可以按照以下步骤操作。这里将涵盖环境设置、依赖添加、API密钥配置以及具体代码实现。### 环境要求1. JDK版本：JDK 17或更高。2. Spring Boot版本：3."
tags: ["Spring","AI","Alibaba","RAG","客服机器人","知识库","代码"]
keywords: ["Spring","AI","Alibaba","RAG","客服机器人","知识库","代码"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么基于自有知识库做rag增强实现客服机器人？  

---------------
## 专家官方解答 ：

根据提供的我了解的信息，要基于Spring AI Alibaba并使用自有知识库实现RAG（检索增强生成）能力的客服机器人，可以按照以下步骤操作。这里将涵盖环境设置、依赖添加、API密钥配置以及具体代码实现。

### 环境要求
1. JDK版本：JDK 17或更高。
2. Spring Boot版本：3.3.x及以上。

### 步骤一：申请阿里云通义千问API Key
- 访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)。
- 登录您的阿里云账号。
- 开通“百炼大模型推理”服务。
- 创建新的API Key，并记录下来。

### 步骤二：配置环境变量
在你的操作系统环境中配置`AI_DASHSCOPE_API_KEY`环境变量，或者直接在`application.properties`文件中指定：
```properties
spring.ai.dashscope.api-key=${YOUR_API_KEY}
```
请确保替换`${YOUR_API_KEY}`为你实际获得的API Key值。

### 步骤三：添加仓库与依赖
由于Spring AI Alibaba仍处于早期阶段，你需要添加特定的Maven仓库以获取相关依赖。同时，在你的`pom.xml`中加入必要的依赖项：

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
    <!-- 其他可能需要的依赖 -->
</dependencies>
```

### 步骤四：编写RAG服务类和控制器
创建一个名为`RagService`的服务类来处理RAG相关的逻辑，包括构建索引和查询文档。接着，通过一个控制器暴露服务端点。

**RagService.java:**

```java
public class RagService {
    // 省略了部分构造器及成员变量定义...
    
    public String buildIndex() {
        // 文件路径示例，请替换成自己的知识库文件路径
        String filePath = "/path/to/your/document.pdf";
        DocumentReader reader = new DashScopeDocumentCloudReader(filePath, dashscopeApi, null);
        List<Document> documentList = reader.get();
        vectorStore.add(documentList);
        return "SUCCESS";
    }

    public StreamResponseSpec queryWithDocumentRetrieval(String message) {
        StreamResponseSpec response = chatClient.prompt().user(message)
                .advisors(new DocumentRetrievalAdvisor(retriever, DEFAULT_USER_TEXT_ADVISE)).stream();
        return response;
    }
}
```

**RagController.java:**

```java
@RestController
@RequestMapping("/ai")
public class RagController {

    private final RagService ragService;

    public RagController(RagService ragService) {
        this.ragService = ragService;
    }

    @GetMapping("/buildIndex")
    public String buildIndex() {
        return ragService.buildIndex();
    }

    @GetMapping("/rag")
    public Flux<String> generate(@RequestParam("message") String message, HttpServletResponse httpResponse) {
        StreamResponseSpec chatResponse = ragService.queryWithDocumentRetrieval(message);
        httpResponse.setCharacterEncoding("UTF-8");
        return chatResponse.content();
    }
}
```

以上步骤完成后，你就成功地利用了Spring AI Alibaba结合自有知识库实现了RAG功能，从而增强了客服机器人的响应质量。记得先调用一次`/ai/buildIndex`接口初始化知识库索引，之后即可通过`/ai/rag`接口进行问题查询。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：Spring ai alibaba 使用RAG能力 
 
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
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17467)给我们反馈。
