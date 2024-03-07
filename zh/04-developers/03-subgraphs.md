# Subgraphs

在 Sofa.org ，我们通过使用 The Graph 提供实时、准确的区块链数据访问功能，从而支持开发者构建高效且响应迅速的应用。Subgraph 是一种强大的工具，允许我们为 Ethereum 上存储的数据创建索引，并通过 GraphQL 查询这些数据。以下是如何利用为 Sofa.org 定制的 subgraphs 来查询相关数据。

## 访问我们的 Subgraph

我们为 Sofa.org 的数据创建了特定的 subgraphs。这些 subgraph 包含从区块链上索引的各种实体，例如交易、账户以及与我们的 DeFi 产品相关的特定事件。你可以通过以下步骤查询 subgraph 中的数据：

### 步骤 1: 找到 Endpoint

找到与你想要交互的 subgraph 对应的 GraphQL endpoint URL。这通常看起来像是一个网址链接，请确保你使用的是最新的 endpoint。

### 步骤 2: 理解 Schema

Schema 描述了 subgraph 中的实体和可以执行的查询类型。理解 schema 对于编写高效的查询至关重要。

### 步骤 3: 编写 GraphQL 查询

根据 schema，你可以开始编写用于查询 subgraph 数据的 GraphQL 查询。GraphQL 使得查询能够非常具体，仅请求你感兴趣的字段。例如，如果你想要查询交易每单的详细信息，你可能会编写像这样的查询：

```
{
  transactions(first: 5) {
    id
    from
    to
    value
    blockNumber
  }
}
```

这将返回区块链上前五个记录的交易及其相关详细信息。

### 步骤 4: 发送查询请求

使用任何支持 GraphQL 的 HTTP 客户端发送你的查询请求。你可以使用命令行工具，如 `curl`，或者集成到你的应用中的库，如 `apollo-client`。发送请求时，通常是以一个 POST 请求发给 endpoint URL。

使用 `curl` 的示例代码如下：

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id from to value blockNumber } }"}' https://api.thegraph.com/subgraphs/name/sofa-org/subgraph-name
```

替换 `subgraph-name` 为真正的 subgraph 名称，URL 根据实际情况来确定。

### 步骤 5: 分析和使用响应

当你的查询请求被发送后，GraphQL 服务会以 JSON 格式返回响应。你可以在你的应用中处理这些数据，或者在前端直接展示给用户。

## 最佳实践

- **使用查询变量**: 如果你经常需要进行相同结构但参数不同的查询，使用查询变量可以更方便地重用你的查询。
- **处理错误**: 除了正常响应，GraphQL 还可以返回错误信息。确保你的客户端代码能够优雅地处理这些情况。

通过以上步骤，你可以有效地使用 The Graph 为你的开发项目查询 Sofa.org 上的数据。更多信息，请参阅我们的开发者文档和 Subgraph 相关资源。

