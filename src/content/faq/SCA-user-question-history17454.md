---
id: "question-history-17454"
title: "给我详细代码和配置： spring ai alibaba 怎么做rag"
date: "2024-11-08"
category: "expertConsultation"
description: "根据我了解的信息中的内容，Spring AI Alibaba 实现检索增强生成（RAG, Retrieval-Augmented Generation）功能主要通过集成阿里云的百炼大模型和私有数据源实现。以下将基于提供的知识介绍如何在Spring AI Alibaba项目中配置和使用RAG功能，包括"
tags: ["Spring","AI","Alibaba","RAG","代码","配置"]
keywords: ["Spring","AI","Alibaba","RAG","代码","配置"]
---

## 用户问题 ： 
 给我详细代码和配置： spring ai alibaba 怎么做rag   

---------------
## 专家官方解答 ：

根据我了解的信息中的内容，Spring AI Alibaba 实现检索增强生成（RAG, Retrieval-Augmented Generation）功能主要通过集成阿里云的百炼大模型和私有数据源实现。以下将基于提供的知识介绍如何在Spring AI Alibaba项目中配置和使用RAG功能，包括代码实现与必要的配置步骤。

### 一、环境准备

确保开发环境满足以下要求：
1. JDK版本至少为JDK 17。
2. 使用Spring Boot版本3.3.x或更高。
3. 在阿里云上申请通义千问API Key。

#### API Key申请流程
- 访问[阿里云百炼页面](https://www.aliyun.com/product/bailian)。
- 登录您的阿里云账号，并开通“百炼大模型推理”服务。
- 开通后，进入个人中心创建新的API Key并记录下来用于后续配置。

### 二、Maven仓库及依赖配置

由于`spring-ai-alibaba-starter`还未发布到正式的Maven中央仓库，您需要添加Spring官方的快照仓库以及里程碑仓库至项目的`pom.xml`文件内：

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

接着，在`<dependencies>`部分添加`spring-ai-alibaba-starter`依赖项：

```xml
<dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter</artifactId>
    <version>1.0.0-M3.1</version>
</dependency>
```

同时设置Spring Boot父项目依赖来启用自动装配机制。

### 三、RAG服务实现

#### 1. RAG服务类定义
创建一个名为`RagService`的服务类，其中包含向量存储、文档检索等核心逻辑。此示例中我们将使用PDF文件作为数据源进行索引构建。

```java
public class RagService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;
    private final DashScopeApi dashscopeApi = new DashScopeApi("YOUR_API_KEY_HERE");
    DocumentRetriever retriever;

    public RagService(ChatClient chatClient, EmbeddingModel embeddingModel) {
        this.chatClient = chatClient;
        vectorStore = new DashScopeCloudStore(dashscopeApi, new DashScopeStoreOptions("my-knowledge-base"));
        retriever = new DashScopeDocumentRetriever(dashscopeApi,
                DashScopeDocumentRetrieverOptions.builder().withIndexName("my-knowledge-base").build());
    }

    // 构建索引方法
    public String buildIndex() {
        String filePath = "/path/to/your/file.pdf";
        DocumentReader reader = new DashScopeDocumentCloudReader(filePath, dashscopeApi, null);
        List<Document> documentList = reader.get();
        vectorStore.add(documentList);
        return "SUCCESS";
    }

    // 查询方法
    public StreamResponseSpec queryWithDocumentRetrieval(String message) {
        return chatClient.prompt().user(message)
                .advisors(new DocumentRetrievalAdvisor(retriever, DEFAULT_USER_TEXT_ADVISE)).stream();
    }
}
```

#### 2. 控制器接口
定义RESTful API控制器以暴露RAG功能。

```java
@RestController
@RequestMapping("/ai")
public class RagController {

    private final RagService ragService;

    public RagController(RagService ragService) {
        this.ragService = ragService;
    }

    @GetMapping("/rag")
    public Flux<String> generate(@RequestParam String message, HttpServletResponse httpResponse) {
        StreamResponseSpec response = ragService.queryWithDocumentRetrieval(message);
        httpResponse.setCharacterEncoding("UTF-8");
        return response.content();
    }

    @GetMapping("/buildIndex")
    public String buildIndex() {
        return ragService.buildIndex();
    }
}
```

### 四、总结说明
上述步骤描述了从环境搭建到具体实现Spring AI Alibaba RAG功能的过程。首先进行了必要的环境配置与依赖管理，然后定义了处理RAG请求的核心服务类`RagService`及其控制层接口`RagController`。这使得应用能够接收用户输入并通过结合外部知识库生成更加准确的答案。请注意替换示例中的占位符（如API密钥路径）为实际值以保证正确运行。


<font color="#949494">---------------</font> 


## 参考链接 ：

* 专家经验：Spring ai alibaba 使用RAG能力 
 
 * 专家经验：sca ai 的prompt用法说明 
 
 * 专家经验：sca ai (spring ai alibaba）介绍 


 <font color="#949494">---------------</font> 
 


## <font color="#FF0000">答疑服务说明：</font> 

本内容经由技术专家审阅的用户问答的镜像生成，我们提供了<font color="#FF0000">专家智能答疑服务</font>,使用方法：
用法1： 在<font color="#FF0000">页面的右下的浮窗”专家答疑“</font>。
用法2： 点击[专家答疑页](https://answer.opensource.alibaba.com/docs/intro)（针对部分网站不支持插件嵌入的情况）
### 另：


有其他开源产品的使用问题？[点击访问阿里AI专家答疑服务](https://answer.opensource.alibaba.com/docs/intro)。
### 反馈
如问答有错漏，欢迎点：[差评](https://ai.nacos.io/user/feedbackByEnhancerGradePOJOID?enhancerGradePOJOId=17468)给我们反馈。
