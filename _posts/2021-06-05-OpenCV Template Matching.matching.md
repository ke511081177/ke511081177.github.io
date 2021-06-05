---
title: "OpenCV筆記 Template matching"
date: 2021-06-5T04:17:30
categories:
  - 筆記
tags:
  - OpenCV
---

 
## 事前雜談 
一直想寫點什麼東西，但每次有東西想寫時寫完都會刪刪改改太多，最後覺得不好又把整篇刪了
，搞得自己很煩躁，於是決定開始不管文章結構，偶爾寫點自己做東西時用到的東西就好
，想到甚麼寫甚麼，畢竟高機率只有我自己會看。


## 原理

- 素材
    1. original img   **(img)**
    2. 要匹配的物件小圖  **(template)**

將**img**和**template**轉成灰階
```
img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
template = cv2.imread('../template.png', 0)
```

，透過template在img上以匹配公式不斷滑動，
以匹配的公式計算誤差，在img上找出跟template最相似的區域，來達成簡易物件追蹤的效果。



## 匹配公式

- OpenCV 有提供這6種


| 匹配誤差計算公式 | 
|--------- |
| cv2.TM_CCOEFF   | 
|cv2.TM_CCOEFF_NORMED   | 
| cv2.TM_CCORR
|cv2.TM_CCORR_NORMED     | 
| cv2.TM_SQDIFF   | 
| cv2.TM_SQDIFF_NORMED    | 



## 範例

- 基本匹配

```
img = cv2.imread("img.png",cv2.IMREAD_UNCHANGED)
img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
template = cv2.imread('../bonus_reward.png', 0)
w, h = template.shape[::-1]

res = cv2.matchTemplate(img_gray, template, cv2.TM_CCOEFF_NORMED)

top_left = max_loc
bottom_right = (top_left[0] + w, top_left[1] + h)
cv2.rectangle(frame,top_left, bottom_right,(0, 0, 255), 2)         ## 畫出匹配到的位置

```

- 匹配(設相似度閥值)

```
img = cv2.imread("img.png",cv2.IMREAD_UNCHANGED)
img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
template = cv2.imread('../bonus_reward.png', 0)
w, h = template.shape[::-1]

res = cv2.matchTemplate(img_gray, template, cv2.TM_CCOEFF_NORMED)
threshold = 0.7                                                     ##閥值可調整
loc = np.where(res >= threshold)
for pt in zip(*loc[::-1]):
    cv2.rectangle(img, pt, (pt[0] + w, pt[1] + h), (0, 0, 255), 2)
```

## 結論

在一些簡單的template圖像蠻好用的，速度又快，但是只要形狀稍稍改變或是大小有縮放就找不太到。
所以如果是ㄧ些複雜的圖形就別試這個了，快去裝YOLO環境，開始標資料比較實在。



