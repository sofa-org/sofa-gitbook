# サブグラフ

私たちは、[The Graph](https://thegraph.com/)のインデックスプロトコルを使用することで、リアルタイムで正確なブロックチェーンデータアクセスを提供し、効率的で応答性の高いアプリケーションの開発をサポートしています。サブグラフは、Ethereumに保存されたデータをインデックス化し、このデータをGraphQLを介してクエリするための強力なツールです。

以下は、カスタムサブグラフを利用してプロトコル内の関連データをクエリする方法です。

| サブグラフ                | クエリURL                                  |
|-------------------------|--------------------------------------------|
| SOFA Mainnet            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA Automator Mainnet  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## サブグラフへのアクセス

私たちはプロトコルのデータのために特定のサブグラフを作成しました。これらのサブグラフには、トランザクション、アカウント、私たちのDeFi製品に関連する特定のイベントなど、ブロックチェーンからインデックス化されたさまざまなエンティティが含まれています。以下の手順に従って、サブグラフ内のデータをクエリできます。

### ステップ1: エンドポイントを見つける

インタラクションしたいサブグラフに対応するGraphQLエンドポイントURLを見つけます。これは通常、ウェブアドレスリンクとして表示されます。最新のエンドポイントを使用していることを確認してください。

### ステップ2: スキーマを理解する

スキーマは、サブグラフ内のエンティティと実行可能なクエリのタイプを説明します。スキーマを理解することは、効率的なクエリを書くために重要です。

### ステップ3: GraphQLクエリを書く

スキーマに基づいて、サブグラフからデータをクエリするためのGraphQLクエリを書き始めることができます。GraphQLは非常に特定のクエリを可能にし、興味のあるフィールドのみを要求します。たとえば、各トランザクションの詳細情報をクエリしたい場合、次のようなクエリを書くことができます：

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
```

これにより、ブロックチェーン上の最初の5件の取引とその関連詳細が返されます。

### ステップ 4: クエリリクエストを送信

GraphQLをサポートする任意のHTTPクライアントを使用してクエリリクエストを送信します。curlのようなコマンドラインツールや、apollo-clientのようなアプリケーションに統合されたライブラリを使用できます。通常、リクエストはエンドポイントURLに対してPOSTリクエストとして送信されます。

curlを使用した例コードは以下の通りです：

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

実際の状況に基づいてURLを置き換えてください。

### ステップ 5: レスポンスを分析して使用する

クエリリクエストが送信されると、GraphQLサービスはJSON形式でレスポンスを返します。このデータをアプリケーション内で処理するか、フロントエンドでユーザーに直接表示できます。

## ベストプラクティス

- **クエリ変数を使用する**: 同じ構造のクエリを異なるパラメータで頻繁に実行する必要がある場合、クエリ変数を使用するとクエリの再利用が容易になります。
- **エラーを処理する**: 通常のレスポンスに加えて、GraphQLはエラー情報も返すことがあります。クライアントコードがこれらの状況を適切に処理できるようにしてください。

上記の手順に従うことで、SOFAプロトコルに関するデータをクエリするためにThe Graphを効果的に使用し、開発プロジェクトをサポートできます。さらなる情報については、残りの開発者ドキュメントやサブグラフ関連リソースを参照してください。
