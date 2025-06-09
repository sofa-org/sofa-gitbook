# オンチェーン決済の考慮事項

## 決済ロジスティクス

> **DeFiの世界におけるジョブスケジューリングの課題**

従来のシステムでは簡単なことですが、**ブロックチェーンにおいては時間指定のデータ取得が難しいです。スマートコントラクトにはネイティブな「Cronのような」ジョブスケジューラがないためです**。暗号通貨において、価格が時間通りにスケジュールされ、更新されることを保証する自動メカニズムをどのように設計できますか？

Basis CashやESDのような暗号「アルゴステーブル」は、DeFiセクターを一時的に席巻し、各エポックの終了時に操作を進め、新しい決済価格を確立するために手動トリガーを必要とする「エポック」間隔アプローチを展開しました。**報酬メカニズムはスマートコントラクトコードに組み込まれ、ボランティアに「進行をトリガーする」よう促し、プロトコルを円滑に運営することを目的としました**。同様に、Andre Cronje（Yearn Finance、Fantom）によって設立された人気プロジェクトKeep3rは、事前リクエストを投稿し、この特定の目的のために指定された仕事の報酬を設定する仮想ジョブボードを作成しました。

どちらの解決策も完璧ではなく、**最終的には人間の介入に依存していました**（自己利益による動機付け）、手動の努力が必要であるため、**実際の最終実行時間に関する不確実性**も考慮しなければなりませんでした。これにより、決済の確定が期限を過ぎてから更新される不快な状況が生じ、満足のいかないユーザー体験につながる可能性があります。

価格更新の定期的なトリガーを呼び出すオフチェーンソリューションを作成し、依存することはできるでしょうか？可能ではありますが、**プロトコルがサーバーの故障などのリスクによって重大な影響を受ける可能性がある中央集権的な「ショートカット」には興味がありません**。

オラクルセクションで説明したように、SOFAは**ChainLinkの自動化サービスを決済価格のソースとして利用しています**。彼らのサービスは、信頼性の高い分散型自動化プラットフォームを通じてスマートコントラクト機能の条件付き実行を可能にし、現在数十億ドルのTVLを確保している外部ノードの運用ネットワークを持っています。

最後に、**最終的な決済価格が確定した後、ユーザーとマーケットメイカーはいつでも契約を呼び出してポジショントークンを焼却し、支払いを受け取ることができます**。この計算は、SOFAの分散型オラクルからの信頼できるデータフィードによってスマートコントラクトによってシームレスに処理されます。

## 支払い計算

製品の支払いを計算する契約は完全に標準化されており、Vaultに依存せず、担保に依存しません。代わりに、**計算契約は基礎となる構造化商品のタイプによって定義され、モデルの支払いを計算します**。いくつかの支払いの例を以下に示します。

### _レンジバウンド_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _スマートトレンド_

#### _ブルトレンド_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _ベアトレンド_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\

X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$