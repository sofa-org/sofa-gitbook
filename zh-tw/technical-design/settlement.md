# 鏈上結算考量

## 結算物流

> **DeFi 世界中的工作排程挑戰**

對於傳統系統來說，這是微不足道的，**時間排程的數據檢索對於區塊鏈來說卻是挑戰，因為智能合約沒有原生的類似 Cron 的工作排程器**。我們如何設計一個自動化機制，以確保加密貨幣的價格能夠按時排程和更新？

加密貨幣的“算法穩定幣”如 Basis Cash 或 ESD 曾經席捲 DeFi 部門，採用了“時代”間隔方法，這需要在每個時代結束時手動觸發以推進操作並建立新的結算價格。**獎勵機制內建於智能合約代碼中，以驅使志願者“觸發推進”**，並保持協議的順利運行。同樣，由 Andre Cronje（Yearn Finance, Fantom）創建的一個名為 Keep3r 的熱門項目創建了一個虛擬工作板，在那裡可以發布推進請求和指定的工作獎勵，專門用於這個目的。

這兩種解決方案都不完美，因為它們**最終依賴於人為干預**（動機自利），更不用說由於需要手動操作而導致的**實際最終執行時間的不確定性**。這可能導致不愉快的情況，即結算固定價格直到到期後很久才更新，導致用戶體驗不佳。

我們能否創建並依賴於鏈下解決方案來定期觸發價格更新？雖然可能，但**我們對集中化的“捷徑”不感興趣**，因為協議可能因伺服器故障等風險而受到嚴重損害。

如我們的預言機部分所述，SOFA 使用**ChainLink 的自動化服務作為我們的結算定價來源**。他們的服務通過可靠且去中心化的自動化平台實現智能合約功能的條件執行，擁有經過驗證的外部節點運營網絡，目前保障著數十億美元的 TVL。

最後，隨著**最終結算價格的確定，用戶和做市商可以隨時調用合約來銷毀他們的頭寸代幣並接收應得的付款**，計算由智能合約無縫處理，並由 SOFA 的去中心化預言機提供可靠的數據源。

## 收益計算

計算產品收益的合約是完全標準化的，且與保險庫無關，對抵押品也不敏感。相反，**計算合約是由結構化產品的基礎類型定義的**，以計算模型的支付。以下列出了一些收益範例：

### _區間限制_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HisghStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _智能趨勢_

#### _牛市趨勢_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _熊市趨勢_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\
X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$