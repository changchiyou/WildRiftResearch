# MVP_SVP

期望能從 [WILD STATS](https://wildstats.gg/) 提供的解包資訊找出 MVP/SVP 的計算公式。


## 分析方向、思路介紹

[WILD STATS](https://wildstats.gg/) 會提供每場對局中的詳細數據，包含大量正常用 WildRift 客戶端看不到的隱藏資訊：

![](https://mblogthumb-phinf.pstatic.net/MjAyMjA0MDZfNTMg/MDAxNjQ5MTczOTA1MTQ0.dfpY4X5e73E5-1NidhEqz5f7o8lLCW95KYhZ5XvJL0Ag.LsuSQvVY0XhkH9FeGKKDhVJvB3HQUH2QL1zer0wBtsgg.PNG.eath96/image.png?type=w800)

<p align="center">（圖片來源：<a href="https://m.blog.naver.com/PostView.naver?blogId=eath96&logNo=222692840716&categoryNo=23&proxyReferer=">모바일롤 와일드리프트 전적검색사이트 전적과 통계및 랭킹 확인</a>）</p>

<br>

除了標準的擊殺、死亡、助攻（KDA）以外，還有像是插眼數量、單場次中不同技能使用次數、甚至發過幾次表情都有被記錄下來（猜測是從重播對戰紀錄取得的資訊）。

其中讓我非常在意的是`Game Points`這項欄位，觀察多場對戰後我得出一個簡單的結論：每場的 MVP/SVP 是由兩隊中`Game Points`最高的玩家得到。

所以分析方向就很明確了，將整頁資訊拉下來，使用相關係數分析、線性回歸嘗試找到一個計算評分的公式，但非常可惜的是 [WILD STATS](https://wildstats.gg/) 在這項研究開始沒多久就關閉了，苦於沒有其他對戰資料的獲取管道只能忍痛廢棄這項研究。

## 直到這研究被廢棄前的研究成果

基本上是半成品 POC，當時只有無腦的把某一場對戰的「所有單場數據」通通丟進去，而非經過數據觀察等人工篩選後再進行計算，這會造成有大量的噪音（noise）干擾計算過程、樣本數也完全不足，因此以下成果只能大略參考。

- 各項數據和`Game Points`的相關係數列表：

    ```
    Damage Taken by Champions                  -0.002476
    Hits using Skill 1                         -0.003567
    Enemies Controlled                          0.005046
    Total Damage Dealt by Abilities             0.013486
    Scryer Blooms Used                         -0.019666
    Rejected Surrender                          0.021694
    Total Damage dealt by Attacks               0.034735
    Total Wards                                 0.034890
    Damage to Champions Dealt with Abilities   -0.040427
    Seconds to Call Rift Herald                -0.062670
    Skill 4 Count                               0.068488
    Total Damage Tanked                         0.096188
    Hits using Skill 2                         -0.098531
    Total Damage Dealt                          0.104798
    Signals                                     0.110349
    Red Buff Kills                              0.111881
    Total Healing Received                     -0.123225
    Hits using Skill 4                          0.125736
    Seconds to First Blood                      0.130162
    Towers Destroyed                            0.159259
    Quick Chats Sent                           -0.166318
    Max Trail Kill Number                      -0.180583
    Wolf Kills                                  0.181275
    Nashor Kills                                0.182951
    Enemies Found using Scryer Blooms           0.188943
    ...
    Gold for Kills                              0.939598
    Game Points                                 1.000000
    Seconds in Game                                  NaN
    dtype: float64
    ```

- 計算`Game Points`用的公式係數：

    ```
    array([-1.16172904e-06, -5.77122457e-08,  3.36336432e-04,  4.37875221e-07,
            1.94147561e-07, -2.41048701e-06,  7.85995160e-08, -1.63949538e-07,
        -3.53169965e-07,  2.63831837e-07,  1.65061417e-07, -3.20540514e-07,
            2.10359492e-07, -7.87095613e-04, -7.71076302e-04, -5.76072427e-04,
        -4.67480329e-04,  1.70818014e-05, -2.44162039e-04, -1.37097586e-03,
            7.16387403e-04, -9.20252856e-04, -5.40461604e-04, -4.86896907e-08,
        -2.44809832e-07,  1.49670955e-07, -7.36797382e-06,  2.79219434e-06,
        -1.47975892e-05,  6.99322286e-05, -3.53169965e-07, -7.93238863e-04,
        -2.03675108e-04, -1.37640452e-03,  8.00361100e-07,  1.38406298e-06,
        -3.03890928e-05,  3.48256800e-07, -9.32696529e-05, -2.24205896e-07,
            1.12916111e-04,  1.22073727e-06,  1.86495706e-07,  1.94357655e-07,
        -4.27148475e-07,  1.37884531e-06, -3.07316186e-07,  2.29695313e-06,
        -2.59439076e-08, -9.27730714e-07, -1.47290692e-06,  2.22962025e-06,
            2.74697693e-07, -7.62211073e-07, -2.38038379e-07, -4.74983672e-08,
        -1.00154992e-07, -3.01072793e-06, -6.77022487e-07, -5.48674973e-04,
        -1.02596021e-04,  1.28390799e-06,  5.69783728e-07, -1.23297993e-05,
        -2.87078788e-05, -2.24049141e-06, -4.55582778e-06,  1.31080270e-06,
            7.70123871e-07, -2.43805572e-06, -1.87118316e-06,  5.39943997e-07,
            4.43805921e-04,  1.53716559e-04, -3.62092523e-05, -2.24205896e-07,
        -3.18335614e-04, -2.77048274e-06,  5.77471625e-07, -3.54554635e-07,
        -2.04472669e-06,  2.71367241e-05, -2.17080884e-05, -7.65188886e-06,
        -7.55637053e-06,  1.60284891e-06, -2.19060158e-07,  3.98528156e-06,
            1.89211916e-07])
    ```