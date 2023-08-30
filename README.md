The content below was converted from [Bahamut_article_backup.bbcode](/MVP_Winrate_Marks/Bahamut_article_backup.bbcode) with [ChatGPT](https://chat.openai.com/).

---

**「elo受益者拉」**
**「什麼爛elo機制讓我一直排到爛隊友」**
**「教你啦，故意不拿MVP就可以靠elo機制上分」**

最近版上充斥的上面三種言論的各種變化形態，我很容易會因為這個議題和別人吵起來，但吵了半天通常到最後不可能產生共識，因為 elo 機制的擁護方通常沒任何數據佐證來讓我信服，我也沒辦法在沒有任何資料的前提下證明一個我認為不存在的東西不存在。

因此從去年年底開始，我開始嘗試花一些閒暇時間研究這塊，到目前為止雖說有一定成果了但還不夠充足，原本想說等研究更深後再專門發一篇文來分享，但前陣子作為我研究資料的 [WILD STATS](https://wildstats.gg/) 的停運公告讓我決定中止研究，直接拿手上的資料發一篇相關的心得分享文，希望能用這種方式回饋版上，讓這篇文作為那些被我一天到晚筆戰煩到的巴友的歉禮：

<img width="828" alt="截圖 2023-08-30 下午3 06 31" src="https://github.com/changchiyou/WildRiftResearch/assets/46549482/66305d4f-c8ee-459e-8893-c75d32ba8e6e">

---

既然主要的研究目標是嘗試反證明「越少拿 MVP 越容易爬分」，不可免的要先提一個東西：[皮尔逊积矩相关系数](https://zh.wikipedia.org/zh-tw/皮尔逊积矩相关系数)。

簡單來說，假設有兩組數據被分別做為（x, y）座標繪製在 2 維座標上，如果今天我需要描述這兩組數據到底有沒有相關、有相關的話具體來說相關到什麼程度時，皮尔逊相關系数 ρ 能協助我能將其量化成數字來表現。

以下圖為例，ρ 的範圍在[-1, 1]之間：

- 當數據完全負相關時（一個數據上升另一個數據就一定會跟著下降）ρ 為 -1
- 當數據完全正相關時（一個數據上升另一個數據就一定會跟著上升）ρ 為 +1
- 當數據完全沒相關時（一個數據上升時另一個數據可能上升或下降）ρ 為 0

![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/3/34/Correlation_coefficient.png/800px-Correlation_coefficient.png)

所以如果「越少拿 MVP 越容易爬分」是真的，那 MVP率不管強弱應該至少和勝率會是負相關才對。

---

有基本概念後就可以參考我在 2023/1/10 統計的 S7 賽季末菁英前 200 數據（官方只有批量公布這部分玩家的數據）。

計算全過程：[GitHub链接](https://github.com/changchiyou/WildRiftResearch/blob/main/MVP_WinRate_Marks.ipynb)

數據（S8-2023-01-17.csv 基本上不可用，因為那時剛開服大家還沒爬上去）：[GitHub链接](https://github.com/changchiyou/WildRiftResearch/blob/main/challenger_datas/S7-2023-01-10.csv)

第一張是 MVP_Ratio（MVP 率）和 Marks（星級）的關係圖和 PPMCC（皮尔逊相關係數），第二張圖則是 MVP_Ratio（MVP 率）和 WinRate（勝率）的關係圖和 PPMCC（皮尔逊相關係數）；MVP 率是拿 MVP 的場次除以總場次，WinRate（勝率）代表爬分速度，Played（總場次）代表花費時間，Marks（星級）代表爬分結果：

MVP Ratio / Marks             |  MVP Ratio / WinRate
:-------------------------:|:-------------------------:
![](/MVP_Winrate_Marks/S7-2023-01-10(1).png)  |  ![](/MVP_Winrate_Marks/S7-2023-01-10(2).png)

可以看到不管是和 Marks（星級）還是 WinRate（勝率），MVP_Ratio（MVP 率）和他們的關係都是弱相關（0＜0.2894, 0.2748＜1+），也就是「基本上無關，但硬要說有那麼一點點正向關係」的意思。也就是說，版上常被某幾位提到的「越少拿 MVP 越容易爬分」的說法，拿菁英前 200 在賽季末的戰績來分析說是錯的，真要說的話拿越多 MVP 爬分效率通常越高。

如果覺得榜前幾位太猛不應該計算在內，這邊有去除掉榜前 10 位的版本：

MVP Ratio / Marks             |  MVP Ratio / WinRate
:-------------------------:|:-------------------------:
