# 2022資料競賽
* 題目：以連續追蹤的心電圖來追蹤左心室射出率的變化

## 比賽網址＆參考網址
* https://linchin.ndmctsgh.edu.tw/app/competition/

## 題目敘述

左心室射出率(left ventricular ejection fraction)是衡量每次收縮時從心臟左心室打出多少血液的百分比，60%的射出率意味著每次心跳都會將左心室總血量的60%排出，而量測左心室射出率的黃金標準是透過心臟超音波(echocardiogram)得到的。

正常心臟的射血分數可能在50%到70%之間，而低於40%的左心室射出率可能暗示著心臟衰竭(heart failure)的狀況。

心電圖已經被證明能夠來估計左心室功能障礙(left ventricular dysfunction)，在使用35,970筆資料的訓練集的支持下，深度學習模型在測試集中可以達到0.932的AUC、86.3%敏感度和85.7%的特異度[Nature Medicine volume 25, pages70–74 (2019)]。

透過連續的心臟超音波追蹤獲得左心室射出率對心臟衰竭的病人而言是非常重要的，這可以有效管理病人狀況並了解治療改善情形，但心臟超音超音波的成本高昂且需要專業人力操作，因此如果能使用心電圖估計左心室射出率，那將有可能讓心臟衰竭的病人受到較密集監測管理，對提升醫療品質有相當大的幫助。

這次任務需要用到心電圖的原始資料，他以csv格式儲存，你需要下載補充資料(153M)，每張心電圖有自己的UID。另外由於這是連續追蹤的真實資料，本此資料中的變項PID是病人編號，測試組中的病人「必定」會出現在訓練資料中有1筆以上的紀錄，而本次預測的重點在於針對這些病人能否給出最後幾筆資料的預測。

訓練資料中有個變項：EF，介於10至90，這是執行該張心電圖後正負7天內實際執行心臟超音波檢查所獲得的左心室射出率結果，而對於同個PID的多個UID來說，他必定是按照時間順序下來，並且每個紀錄的時間間格都超過14天，以避免不同心電圖共享單次心臟超音波結果。

訓練資料共有9,806筆資料，而測試資料共有4,162筆資料(866筆用在open leaderboard，3,296筆用在private leaderboard，此資訊為隱藏資訊)，為了降低入門門檻，提交的結果預測是以0-1之間的機率為準，數字越大代表病人的EF越有可能在35%以下，而結果的評估指標是使用ROC曲線分析中的AUC，數字0.5代表幾乎沒有預測能力，而數字1代表著完美的效能。

本次資料科學競賽的資料集由三軍總醫院提供，第一名可以獲得獎金新台幣50,000元，第二名可以獲得獎金新台幣30,000元，第三名可以獲得獎金新台幣20,000元，排名以最後一筆上傳至未公開的private leaderboard為準。

本年度會在111年7月4日正式開始競賽並接受提交答案，競賽期間至111年8月29日為止，並在111年10月22日(六)於研討會中頒獎，前三名的隊伍需要至研討會中分享自己的作法，如無法分享實際做法則名次依序遞補。

每張心電圖的都是標準12導程心電圖，原始資料是以500採樣頻率採樣10秒，但因為資料量的問題壓縮至100Hz，因此每個lead都有1,000個數值序列，其數值單位是0.01mv。

## 作法

方法一：
