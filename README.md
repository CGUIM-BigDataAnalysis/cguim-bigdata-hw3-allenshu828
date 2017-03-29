長庚大學 大數據分析方法 作業三
================

網站資料爬取
------------

``` r
library(rvest) ##第一次使用要先安裝
```

    ## Loading required package: xml2

``` r
##read_html
tmp <- "https://www.ptt.cc/bbs/NBA/index"
totalFrame <- NULL
for(i in 4627:1){
  PTT_NBAUrl <- paste0(tmp, as.character(i), ".html")
  NBAContent <- read_html(PTT_NBAUrl)
  NBAPostTitle <- NBAContent %>%  html_nodes(".title") %>% html_text() 
  NBAPostAuthor <- NBAContent %>% html_nodes(".author") %>% html_text()
  NBAPostPushNum <- NBAContent %>% html_nodes(".nrec") %>% html_text()
  tmpFrame <- data.frame(Order = c(1:length(NBAPostTitle)), ## for ranking the post according post time
                         Title = NBAPostTitle, 
                         Author = NBAPostAuthor,
                         PushNum = NBAPostPushNum)
                         #Url = PTT_NBAUrl
  tmpFrame <- tmpFrame[order(tmpFrame$Order, decreasing = T),] ## reverse the order the tmpFrame
  rownames(tmpFrame) <- c(1:nrow(tmpFrame)) ## reassign the rownames()
  
  totalFrame <- rbind(totalFrame, tmpFrame) ## integrate tmpFrame to totalFrame
  if(nrow(totalFrame) > 100){ ## over 100 post
    break
  }
}
totalFrame$Order <- NULL ## delete the Order column


##html_nodes 
##html_text
```

爬蟲結果呈現
------------

``` r
#這是R Code Chunk
knitr::kable(totalFrame) ##請將iris取代為上個步驟中產生的爬蟲資料資料框
```

| Title                                                 | Author       | PushNum |
|:------------------------------------------------------|:-------------|:--------|
| \[討論\] 穩進季後賽的火箭，仍然最多一輪               | sunnycello   | X3      |
| \[討論\] 留KD或Curry？                                | star1739456  | 爆      |
| \[新聞\] 林書豪在場能量加倍 籃網輸球也精彩            | lcall        | 2       |
| \[情報\] Kerr fastest coach in American sports        | Angel0724    | 72      |
| Re: \[討論\] 去年快艇如何守哈登買飯集錦               | bluestaral   | 17      |
| \[討論\] 沒KD 勇士還是不可小看                        | feyako       | 13      |
| \[新聞\] 勇士「浪花兄弟」開轟　火箭哈登熄火吞敗       | zzzz8931     | 34      |
| \[BOX \] Warriors 113:106 Rockets 數據                | Rambo        | 爆      |
| \[新聞\] 林書豪關鍵走步失誤　籃網惜敗76人             | jhemezuo     | 29      |
| \[Live\] 巫師 @ 湖人                                  | Rambo        | 34      |
| \[BOX \] Heat 97:96 Pistons 數據                      | Rambo        | 21      |
| \[新聞\] 上場時間決定MVP？　哈登：我從未缺席比        | zzyyxx77     | 27      |
| \[BOX \] Suns 91:95 Hawks 數據                        | Rambo        | 20      |
| \[BOX \] Sixers 106:101 Nets 數據                     | Rambo        | 91      |
| \[BOX \] Timberwolves 115:114 Pacers 數據             | Rambo        | 51      |
| \[Live\] 金塊 @ 拓荒者                                | Rambo        | 44      |
| \[BOX \] Bucks 118:108 Hornets 數據                   | Rambo        | 37      |
| \[討論\] 籃網RHJ這個球員......                        | ronharper    | 23      |
| \[新聞\] 年度教練競爭激烈 柯爾力挺丹東尼              | pneumo       | 30      |
| \[Live\] 勇士 @ 火箭                                  | Rambo        | 96      |
| (已被kadasaki刪除) <HANASUCIA> OP                     | -            | X5      |
| \[Live\] 熱火 @ 活塞                                  | Rambo        | 12      |
| \[Live\] 太陽 @ 老鷹                                  | Rambo        | 1       |
| \[Live\] 七六人 @ 籃網                                | Rambo        | 爆      |
| \[Live\] 灰狼 @ 溜馬                                  | Rambo        | 16      |
| \[Live\] 公鹿 @ 黃蜂                                  | Rambo        | 4       |
| Re: \[新聞\] 好腰高難度空接　Manu：看不懂他怎麼傳     | steveparker  | 16      |
| \[討論\] 球員綽號                                     | ZaneTrout    | 20      |
| \[情報\] 火箭差8顆三分打破單季三分進球數紀錄          | Wall62       | 38      |
| Re: \[討論\] NBA球員有拿過大滿貫的?                   | Jachu        | 1       |
| \[公告\]多人水桶                                      | namie810303  | 爆      |
| \[新聞\] 女性總教練？ 主席席佛：希望快點出現          | Gotham       | 45      |
| Re: \[討論\] 之前有球隊刻意輸比賽 控制對手嗎          | BBDurant     | 1       |
| \[情報\] 微笑刺客：若Rose最終去了馬刺不會讓我感       | tmacor1      | 爆      |
| \[新聞\] 馬刺大勝騎士 雷納德：沒有任何意義            | ccps970915   | 59      |
| \[新聞\] NBA／尼克將改變策略？ 何納塞克：下季將       | iam168888888 | 28      |
| \[新聞\] 好腰高難度空接　Manu：看不懂他怎麼傳         | zzzz8931     | 69      |
| (本文已被刪除) \[MrSatan\]                            | -            | 1       |
| Re: \[情報\] 甜瓜：禁藥名單太長，我會選擇中草藥       | tadshift2    | 2       |
| \[討論\]John Wall 算東區第一控了嗎？                  | h1212123tw   | 99      |
| \[情報\] 控衛防守哪家強？ “Wall”成聯盟第一            | tmac0818     | 76      |
| \[花邊\] Kyrie Irving賽後獨自加練                     | KyrieIrving1 | 80      |
| (本文已被刪除) \[hawoo\]                              | -            |         |
| \[花邊\] Steve Francis, Joe Smith 加入BIG3聯賽        | thnlkj0665   | 23      |
| Re: \[討論\] NBA球員有拿過大滿貫的?                   | Myosotis     | 29      |
| \[新聞\] 隊友藥檢未 甜瓜：我都呷中藥顧健康            | royhsu425    | 56      |
| (本文已被刪除) \[goodbad\]                            | -            | 15      |
| (已被kadasaki刪除) <knic52976> 1-2                    | -            | X1      |
| Re: \[討論\] 東西勝場差～這些年的西強東弱             | MK12         | 25      |
| (已被namie810303刪除) <OGC789456123>引戰              | -            | X3      |
| \[情報\] Kobe：Booker有我想傳承給下一代球員的         | lovea        | 50      |
| Re: \[討論\] NBA球員有拿過大滿貫的?                   | monmo        | 19      |
| \[專欄\] 死豬不怕開水燙 尼克丟臉已成習慣              | zzyyxx77     | 19      |
| \[新聞\] 騎士近況低迷 詹姆斯：得找出解決方法          | adam7148     | 61      |
| \[討論\] 東西勝場差～這些年的西強東弱                 | liusim       | 78      |
| \[討論\] NBA球員有拿過大滿貫的?                       | chchang0820  | 28      |
| Re: \[情報\] NBA Standings(2017.03.28)                | cric335      | 19      |
| Re: \[討論\] Lebron被大衛李尻背部受傷???              | iammatrix    | 18      |
| \[情報\] 甜瓜：禁藥名單太長，我會選擇中草藥           | Yui5         | 57      |
| Re: \[討論\] 如果騎士在西區                           | Turtle100    | X1      |
| Re: \[情報\] NBA Standings(2017.03.28)                | kadasaki     | 爆      |
| \[討論\] Lebron被大衛李尻背部受傷???                  | chouvincent  | 99      |
| \[情報\] Kawhi Leonard連續100場得分雙位數             | jimmy5680    | 37      |
| \[情報\] 詹姆斯：我想讓我們能打得更好些               | knic52976    | 32      |
| \[討論\] 之前有球隊刻意輸比賽 控制對手嗎              | peace9527    | 22      |
| \[新聞\] 季中吞大補丸卻慘敗馬刺 詹皇寫一項最難        | dameontree   | 31      |
| \[BOX \] Grizzlies 90:91 Kings 數據                   | hungys       | 39      |
| \[新聞\] 衛少第37次大三元 致勝一擊逆轉小牛            | MLbaseball   | 30      |
| \[BOX \] Pelicans 100:108 Jazz 數據                   | Rambo        | 25      |
| Re: \[新聞\] 老大拒絕輪休 卻認為詹皇是例外            | ICHIKOnice   |         |
| (本文已被刪除) <AsakuraYume>                          | -            | 18      |
| \[新聞\] 騎士慘敗給馬刺　跌下東區龍頭寶座             | JAL96        | 26      |
| \[新聞\] 球迷為搏版面出狠招 不惜拿兒子祭旗            | abc7360393   | 14      |
| \[討論\] 騎士是不是為了避開熱火或公牛？               | Max11        | 17      |
| \[新聞\] 林書豪教得好 隊友會用中文說贏球              | Assisi       | 37      |
| \[新聞\] 老大拒絕輪休 卻認為詹皇是例外                | pttpepe      | 75      |
| \[討論\] 騎士球隊CP值                                 | t13thbc      | 21      |
| \[討論\] 這場結束之後 是不是確定了西強東弱？          | tiffanycute  | 爆      |
| Re: \[討論\] 騎士東區第一的位置是不是不保了           | j5826497110  | 4       |
| (已被namie810303刪除) <a102203028>引戰退              | -            |         |
| (已被namie810303刪除) <Lankavatara>1-6                | -            | 37      |
| Re: \[討論\] 騎士東區第一的位置是不是不保了           | kingrichman  | 35      |
| \[BOX \] Thunder 92:91 Mavericks 數據                 | Rambo        | 68      |
| \[新聞\] 馬刺痛宰騎士收5連勝 東部龍頭換人當           | pneumo       | 21      |
| \[討論\] 騎士是 真藏招 還是 還在 磨合 ?               | Turtle100    | 75      |
| \[BOX \] Cavaliers 74:103 Spurs 數據                  | Rambo        | 爆      |
| \[討論\] 騎士東區第一的位置是不是不保了               | CurryMvp     | 7       |
| (本文已被刪除) \[a10141013\]                          | -            | 1       |
| \[新聞\] 不滿快艇遮湖人 歐尼爾嗆別擋住老子球衣        | meiyouo      | 35      |
| \[BOX \] Magic 112:131 Raptors 數據                   | Rambo        | 12      |
| \[Live\] 鵜鶘 @ 爵士                                  | Rambo        | 4       |
| \[BOX \] Pistons 95:109 Knicks 數據                   | Rambo        | 30      |
| \[花邊\] O'Neal：我曾給餐廳服務生付過四千美元小費     | bigDwinsch   | 55      |
| \[花邊\] Kobe退休之後　最愛看這4名球星打球            | pneumo       | 64      |
| \[情報\] 湖人Buss家族內鬥結束,Jeanie掌權              | sezna        | 9       |
| \[花邊\] Duncan：每次在家看到馬刺贏球我都會罵人       | Yui5         | 爆      |
| \[情報\] 國王有意撤換總管Vlade Divac                  | dragon803    | 25      |
| \[情報\] 東西區上週最佳球員：DeRozan, Harden          | Maxwell5566  | 19      |
| \[Live\] 雷霆 @ 小牛                                  | Rambo        | 74      |
| \[討論\] 騎馬大戰                                     | jojoii82     | 2       |
| \[Live\] 騎士 @ 馬刺                                  | Rambo        | 爆      |
| \[Live\] 活塞 @ 尼克                                  | Rambo        | 6       |
| \[Live\] 魔術 @ 暴龍                                  | Rambo        | 2       |
| \[情報\] Shumpert&KK今天不會上場                      | Wall62       | 19      |
| Re: \[花邊\] Vanessa曝Kobe虛報身高，<U+8131>鞋只有195 | Wall62       | 1       |
| \[討論\] 助攻認定是何時變寬鬆的？                     | ronharper    | 8       |
| \[討論\] 在MVP的光環下，失誤是芝麻小事不值一提        | checktime    | 13      |
| Re: \[外絮\] 有人公布了球爸打全場的影片               | barsax8      | 14      |
| \[討論\] ok連線湖人和kb+gasol的湖人，哪個強？         | s106667      | 27      |
| \[討論\] 近幾年還有哪些隊是大熔爐隊伍?                | KyrieIrving1 | 5       |
| \[花邊\] 沒有走步！ 魏少小碎步跳投引熱議（影音)       | Gotham       | 44      |
| (本文已被刪除) \[dragon803\]                          | -            | 2       |
| \[公告\]多人水桶                                      | namie810303  | 爆      |
| \[花邊\] 球星輪休爭議 馬里安：球員要為球迷而戰        | lovea        | 12      |
| \[新聞\] 抨擊養生策略 Beverley：球迷該得到更多        | iam168888888 |         |
| \[新聞\] 哈登才是MVP！ 火箭推特嗆：大三元只是數字     | Gotham       | 91      |
| \[討論\] 昔日雷霆三少                                 | starcraftiii | 36      |
| \[外絮\] 有人公布了球爸打全場的影片                   | huntai       | 35      |
| \[情報\] ★今日排名(2017.03.27)★                       | Rambo        | 20      |
| (已被kadasaki刪除) <ilovesumika> 1-2                  | -            | X1      |

解釋爬蟲結果
------------

``` r
#這是R Code Chunk
# - 用文字搭配程式碼解釋爬蟲結果
#    - 共爬出幾篇文章標題？（程式碼與文字解釋各`5pt`）
#        - dim(), nrow(), str()皆可
com1 <- paste0("A total of  ", as.character(nrow(totalFrame)), " titles were acquired from PTT/NBA.")
print(com1)
```

    ## [1] "A total of  120 titles were acquired from PTT/NBA."

``` r
#    - 哪個作者文章數最多？（程式碼與文字解釋各`5pt`）
#        - table()
table.fre.author <- sort(table(totalFrame$Author, dnn = "AuthorName"), decreasing = T)
com2 <- paste0("Author who posted the most is ", attr(table.fre.author[1], "names"), ".")
print(com2)
```

    ## [1] "Author who posted the most is Rambo."

A total of 120 titles were acquired from PTT/NBA.

Author who posted the most is Rambo.

``` r
#這是R Code Chunk
#    - 其他爬蟲結果解釋（`10pt`）
#        - 試著找出有趣的現象，不一定要用程式碼搭配解釋，也可只用文字
totalFrame.TheMostAuthor <- subset(totalFrame, Author == attr(table.fre.author[1], "names"))
knitr::kable(totalFrame.TheMostAuthor)
```

|     | Title                                     | Author | PushNum |
|-----|:------------------------------------------|:-------|:--------|
| 8   | \[BOX \] Warriors 113:106 Rockets 數據    | Rambo  | 爆      |
| 10  | \[Live\] 巫師 @ 湖人                      | Rambo  | 34      |
| 11  | \[BOX \] Heat 97:96 Pistons 數據          | Rambo  | 21      |
| 13  | \[BOX \] Suns 91:95 Hawks 數據            | Rambo  | 20      |
| 14  | \[BOX \] Sixers 106:101 Nets 數據         | Rambo  | 91      |
| 15  | \[BOX \] Timberwolves 115:114 Pacers 數據 | Rambo  | 51      |
| 16  | \[Live\] 金塊 @ 拓荒者                    | Rambo  | 44      |
| 17  | \[BOX \] Bucks 118:108 Hornets 數據       | Rambo  | 37      |
| 20  | \[Live\] 勇士 @ 火箭                      | Rambo  | 96      |
| 22  | \[Live\] 熱火 @ 活塞                      | Rambo  | 12      |
| 23  | \[Live\] 太陽 @ 老鷹                      | Rambo  | 1       |
| 24  | \[Live\] 七六人 @ 籃網                    | Rambo  | 爆      |
| 25  | \[Live\] 灰狼 @ 溜馬                      | Rambo  | 16      |
| 26  | \[Live\] 公鹿 @ 黃蜂                      | Rambo  | 4       |
| 69  | \[BOX \] Pelicans 100:108 Jazz 數據       | Rambo  | 25      |
| 83  | \[BOX \] Thunder 92:91 Mavericks 數據     | Rambo  | 68      |
| 86  | \[BOX \] Cavaliers 74:103 Spurs 數據      | Rambo  | 爆      |
| 90  | \[BOX \] Magic 112:131 Raptors 數據       | Rambo  | 12      |
| 91  | \[Live\] 鵜鶘 @ 爵士                      | Rambo  | 4       |
| 92  | \[BOX \] Pistons 95:109 Knicks 數據       | Rambo  | 30      |
| 99  | \[Live\] 雷霆 @ 小牛                      | Rambo  | 74      |
| 101 | \[Live\] 騎士 @ 馬刺                      | Rambo  | 爆      |
| 102 | \[Live\] 活塞 @ 尼克                      | Rambo  | 6       |
| 103 | \[Live\] 魔術 @ 暴龍                      | Rambo  | 2       |
| 119 | \[情報\] ★今日排名(2017.03.27)★           | Rambo  | 20      |

``` r
print("The posts by Rambo include Box, Live, and Standing.")
```

    ## [1] "The posts by Rambo include Box, Live, and Standing."

The posts by Rambo include Box, Live, and Standing.
