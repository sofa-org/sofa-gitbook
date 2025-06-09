
# 子图

我们通过使用 [The Graph](https://thegraph.com/) 的索引协议，支持高效和响应迅速的应用程序开发，提供实时和准确的区块链数据访问。子图是一个强大的工具，允许我们索引存储在以太坊上的数据，并通过 GraphQL 查询这些数据。

以下是如何利用自定义子图在协议内查询相关数据。

| 子图                     | 查询 URL                                   |
|-------------------------|--------------------------------------------|
| SOFA 主网               | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA 自动化主网         | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA 自动化 Arbitrum    | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## 访问我们的子图

我们为协议的数据创建了特定的子图。这些子图包含从区块链索引的各种实体，例如交易、账户以及与我们的 DeFi 产品相关的特定事件。您可以通过以下步骤在子图中查询数据：

### 步骤 1：找到端点

找到与您希望交互的子图对应的 GraphQL 端点 URL。它通常显示为一个网页链接。请确保您使用的是最新的端点。

### 步骤 2：了解模式

模式描述了子图中的实体以及可以执行的查询类型。了解模式对于编写高效查询至关重要。

### 步骤 3：编写 GraphQL 查询

根据模式，您可以开始编写 GraphQL 查询以从子图中查询数据。GraphQL 允许进行非常具体的查询，只请求您感兴趣的字段。例如，如果您想查询每笔交易的详细信息，您可以编写如下查询：

```
{
  transactions(first: 5) {
    vault
    tradingFeeRate
    totalCollateral
    timestamp
    term
    spreadAPR
    referral
    minter
    makerCollateral
    maker
    id
    hash
    expiry
    collateralAtRiskPercentage
    borrowAPR
    anchorPrices
  }
}
```

这将返回区块链上前五条记录的交易及其相关详细信息。

### 第4步：发送查询请求

使用任何支持 GraphQL 的 HTTP 客户端发送查询请求。您可以使用命令行工具如 curl，或集成到应用程序中的库，如 apollo-client。通常，请求作为 POST 请求发送到端点 URL。

使用 curl 的示例代码如下：

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

根据实际情况替换 URL。

### 第5步：分析和使用响应

发送查询请求后，GraphQL 服务将以 JSON 格式返回响应。您可以在应用程序中处理这些数据或直接在前端显示给用户。

## 最佳实践

- **使用查询变量**：如果您经常需要执行结构相同但参数不同的查询，使用查询变量可以更容易地重用您的查询。
- **处理错误**：除了正常响应外，GraphQL 还可以返回错误信息。确保您的客户端代码能够优雅地处理这些情况。

通过遵循上述步骤，您可以有效地使用 The Graph 查询 SOFA 协议上的数据，以支持您的开发项目。请参阅我们的开发者文档和子图相关资源以获取更多信息。
