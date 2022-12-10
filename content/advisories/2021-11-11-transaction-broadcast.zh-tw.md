
+++
title = "公告：對交易自動重新進行的回應"
date = "2021-11-11"
[ author ]
  name = "Dogecoin Foundation"
+++

注意：本篇公告的前個版本使用了＂我們＂來代指獨立於基金會的一群成員。為能更清楚地說明，這篇公告已被編修過。

幣安（Binance）今日暫停了狗狗幣的提款功能，並指出他們在狗狗幣發現＂小問題＂。對此，基金會想做出回應：

數個月前（請注意，在稍早發文中說的是一年前，但首次確認的提及是在四月）幣安知會了 Dogecoin Core 專案的一群維護者，說明幣安遇到一些交易卡住的情況，交易無法成功被礦工打包。維護者們建議幣安對這些交易使用 RBF（Replace-By-Fee）機制 - 以使用較高手續費的新交易取代原先的交易。值得重視的是，RBF 能使先前的交易失效（因此也可說是＂取代＂），所以這個作法被優先建議。由於那些交易沒有啟用 RBF，另一個建議作法也被提出：手動建立使用相同 input 的新交易，強制使原先的交易失效。

一段時間之後，幣安向維護者們表示他們遇到帳目核對問題。維護者們無法用幣安提供的資料重現那些狀況，但建議了使用 `-zapwallettxes` 命令列指令。這是值得注意的，因為預計這個作法可以防止該問題。

昨日（11月10日）幣安告知維護者們：在 Dogecoin Core 1.14.5 更新後，幣安先前卡住的交易都突然成功運行了。可能是因為 1.14.5 版本中交易手續費已被降低，致使之前有效但不被運行的交易被運行了。幣安提供的唯一一個案例，是手續費自 1.14.5 起有效，但在 1.14.3 以前無效（因為太低）的一筆交易。請注意幣安在過去幾天內已從 1.14.3 直接升級為 1.14.5。

根據目前所知的資訊，這些情況似乎是之前卡住的交易自動被重新嘗試執行了，就像節點在升級版本後重啟時會發生的一樣 - 並且因為目前最低需求手續費已經降低而成功。這是由於交易手續費降低所導致的，正確的現象。

## 學到的一課
* 正確地取消交易：欲取消的交易和新的交易應使用相同 input 以使前者失效。
  * 如果可以，使用 Replace-By-Fee 是更理想的作法。但手動建立一個使用相同 input 的新交易也能使先前的交易失效。
* 請注意交易並沒有預定的逾時時限（timeout），但通常都會因記憶體限制而被終止。

## 指引

基金會沒有再收到該情況發生的回報。交易服務提供者若對停滯的無效交易有疑慮，我們建議停止節點，為以防萬一也請移除 mempool.dat 檔案，然後再以 `-zapwallettxes` 指令啟動節點。

個人用戶若有疑慮，請注意，將你的狗狗幣全數提出（理想情況是使用一個新的錢包地址，但也可以使用現有的）也能花費掉先前所有交易 output，並使那些＂卡住＂的交易失效。