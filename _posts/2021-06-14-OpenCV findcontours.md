---
title: "OpenCV筆記 我本以為你可以: Contour Searching"
date: 2021-06-14T19:20:30
categories:
  - 筆記
tags:
  - OpenCV
---
###### tags: `blog`
 
## 事前雜談 
這個東西是在做楓之谷的測謊機偵測的時候想拿來抓測謊機位置的，不過用來用去這個方法還是以失敗告終，乖乖去標資料訓練模型比較實在，總歸應該還是自己影像前處理的能力不足，尤其是在做嘗試的時候常常結果不好的時候很爆氣。


## 原理

沒有深究。

## 步驟

1. 把照片轉成灰階
    `python
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    `

2. 找輪廓
     `python
     contours,_ = cv2.findContours(gray.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
     `
 
3. 看需求: 要找位置或是框出來的話可以這樣
```python
for c in contours:
            
    x,y,w,h=cv2.boundingRect(c)
    x=int(((2*x+w)/2)-10)
    y=int(((2*y+h)/2)-10)

    if (w and h) > 7 and y >0 and x>0: 

        cv2.rectangle(img,(x,y),(x+w,y+h),(0,255,0),1)#绿色框框
```
可以用這種方式
- 抓輪廓的座標
- 如果要抓一定大小以上的輪廓的話，用這個來設定閥值
- 
## function參數

cv2.findContours(1.灰階圖片, 2.要取得怎麼樣的輪廓, 3.回傳的輪廓pixel點格式)

1. 不解釋

2. 就是，要取得怎樣的輪廓
    -    cv2.RETR_LIST：選到的輪廓不建立上下階層關係
    -    cv2.RETR_TREE：建立一個樹狀結構的輪廓
    -    cv2.RETR_CCOMP：建立內外兩層的輪廓階層
    -    cv2.RETR_EXTERNAL：只檢測外側輪廓


3. 儲存的輪廓訊息
    -    cv2.CHAIN_APPROX_NONE : 這小子存了幾乎所有的點
    -    cv2.CHAIN_APPROX_SIMPLE : 比起第一個，減少了存儲的點數量，壓縮了一些一直線的座標。
    -    還有這兩位: cv2.CHAIN_APPROX_TC89_L1，CV_CHAIN_APPROX_TC89_KCOS，不過沒用到所以沒研究。

4. 涵式回傳 contours,_
    - contours : 一個np array，有每一個輪廓的儲存值在裡面
    -  _ : 如果前面參數選擇的是有階層的輪廓找尋，也是回傳一個np array，包含每一個輪廓的後、前、上層、內部輪廓的編號。

    
## 範例

- 前面的彙整
```python

    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    contours,_ = cv2.findContours(gray.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
    for c in contours:
            
    x,y,w,h=cv2.boundingRect(c)
    x=int(((2*x+w)/2)-10)
    y=int(((2*y+h)/2)-10)

    if (w and h) > 7 and y >0 and x>0: 
        cv2.rectangle(img,(x,y),(x+w,y+h),(0,255,0),1)#绿色框框
        
```

## 結論

在圖像複雜的時候如果甚麼前處理都不做的話，就等著輸出拿到一堆大便，需要先做一些處理來讓灰階後的圖像辨識度高一點。



