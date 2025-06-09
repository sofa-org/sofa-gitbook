# 子圖

我們透過使用 [The Graph](https://thegraph.com/) 的索引協議，提供即時且準確的區塊鏈數據訪問，支持高效且響應迅速的應用程式開發。子圖是一個強大的工具，允許我們索引存儲在以太坊上的數據，並通過 GraphQL 查詢這些數據。

以下是如何利用自定義子圖來查詢協議內的相關數據。

| 子圖                    | 查詢 URL                                    |
|-------------------------|--------------------------------------------|
| SOFA Mainnet            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA Automator Mainnet  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## 訪問我們的子圖

我們為協議的數據創建了特定的子圖。這些子圖包含從區塊鏈索引的各種實體，例如交易、賬戶和與我們的 DeFi 產品相關的特定事件。您可以通過以下步驟在子圖中查詢數據：

### 步驟 1：找到端點

找到您希望互動的子圖對應的 GraphQL 端點 URL。這通常顯示為一個網址鏈接。請確保您使用的是最新的端點。

### 步驟 2：了解架構

架構描述了子圖中的實體以及可以執行的查詢類型。了解架構對於編寫高效的查詢至關重要。

### 步驟 3：編寫 GraphQL 查詢

根據架構，您可以開始編寫 GraphQL 查詢以從子圖中查詢數據。GraphQL 允許非常具體的查詢，只請求您感興趣的字段。例如，如果您想查詢每筆交易的詳細信息，您可能會編寫如下查詢：

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

這將返回區塊鏈上前五筆記錄的交易及其相關詳細信息。

### 步驟 4：發送查詢請求

使用任何支持 GraphQL 的 HTTP 客戶端發送您的查詢請求。您可以使用命令行工具如 curl，或集成到您的應用程式中的庫，如 apollo-client。通常，請求以 POST 請求的形式發送到端點 URL。

使用 curl 的示例代碼如下：

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

根據實際情況替換 URL。

### 步驟 5：分析和使用響應

在您的查詢請求發送後，GraphQL 服務將以 JSON 格式返回響應。您可以在您的應用程式中處理這些數據或直接在前端顯示給用戶。

## 最佳實踐

- **使用查詢變量**：如果您經常需要執行結構相同但參數不同的查詢，使用查詢變量可以使您的查詢更易於重用。
- **處理錯誤**：除了正常響應外，GraphQL 還可以返回錯誤信息。確保您的客戶端代碼能夠優雅地處理這些情況。

通過遵循上述步驟，您可以有效地使用 The Graph 查詢 SOFA 協議上的數據，以支持您的開發項目。請參閱我們的開發者文檔和子圖相關資源以獲取更多信息。