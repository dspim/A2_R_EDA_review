---
title       : R basic and EDA Review
subtitle    : 智庫驅動
author      : Ben Chen
job         : 
framework   : io2012-dsp
highlighter : highlight.js
hitheme     : zenburn
widgets     : [quiz, bootstrap]           # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- .dark .segue

## R basic



--- .largecontent
## 變數賦值


1. 變數的賦值方式： <- (箭號) 以及 = (等號)， 建議使用 <- 。
2. 註解以 # (井號) 表示。
3. R 物件最基本的單位是向量 (vector)，以 c() 表示 (c 取自combine之意)，元素以逗號分隔，其中向量包含三種基本類別(class)：
  - 數值向量 (numeric vector)
  - 字串向量 (character vector)
  - 布林向量 (logical vector)

--- &twocol
## 變數賦值
*** =left

```r
# Numeric，class(x)查詢物件的類別
x <- c(1.1, 2.2, 5.23)
class(x)
```

```
## [1] "numeric"
```



```r
# Character
y <- c("apple", "book", "cat")
class(y)
```

```
## [1] "character"
```

*** =right

```r
# logical
z <- c(TRUE, FALSE, TRUE)
class(z)
```

```
## [1] "logical"
```

--- &twocol
## 向量元素的命名
在 R 語言中，可以對向量中的每一個元素命名，或者是利用函數 names 對向量元素命名，這有助於該向量的理解。

*** =left

```r
y <- c("apple", "book", "cat")
y1 <- c(A = "apple", B = "book", C = "cat")
y1
```

```
      A       B       C 
"apple"  "book"   "cat" 
```


*** =right

```r
y <- c("apple", "book", "cat")
names(y1) <- c("A", "B", "C")
y1
```

```
##       A       B       C 
## "apple"  "book"   "cat"
```

```r
names(y1)
```

```
## [1] "A" "B" "C"
```


--- &radio

## 練習

如何以y對z命名？

    y <- c("apple", "book", "cat")
    z <- c(TRUE,FALSE,TRUE)

1. name(z)<-y
2. names(y)<-z
3. _names(z)<-y_

*** .hint
names

*** .explanation
names(z)<-y
z

---
## factor物件簡介

當一向量變數是類別型變數 (categorical data，譬如：性別、教育水準) 時，在R語言中以factor進行定義。

```r
# 產生2個'male'和3個'female'
gender <- c(rep("male", 2), rep("female", 3))  #rep: 複製value
gender
```

```
## [1] "male"   "male"   "female" "female" "female"
```

```r
gender <- factor(gender)  #利用factor將字串變成factor
gender
```

```
## [1] male   male   female female female
## Levels: female male
```


--- &twocol
## Level的順序
*** =left

```r
levels(gender)
```

```
## [1] "female" "male"
```

```r
as.numeric(gender)
```

```
## [1] 2 2 1 1 1
```

```r
# 1=female, 2=male 依字母順序排列
```

*** =right
![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 

--- &vcenter .largecontent
# Change vector of labels for the levels


```r
gender <- factor(gender, levels = c("male", "female"), labels = c("M", "F"))
gender  # 改變了level的順序，也改變了label名稱
```

```
## [1] M M F F F
## Levels: M F
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 

---
## 練習factor

請將city以以下的順序為level的順序設為factor物件

    city <- c('Taipei','DC','Tokyo','London')

1. _city<-factor(city, levels=city)_
2. city<-factor(city)
3. city<-factor(city, labels=city)

*** .hint
應該控制levels

*** .explanation
city<-factor(city, levels=city)
city

---
## 讀取表格檔案

```r
# 選擇路徑
path <- file.choose()

# 先讀前5列觀察數據
ubike <- read.table(path, sep = ",", header = TRUE, nrows = 5)
head(ubike)

# 利用read.table讀取檔案，要注意資料分隔方式，csv檔通常以逗號分隔
# sep表示分隔符號，header表示是否將第一列視為欄位名稱
ubike <- read.table(path, sep = ",", header = TRUE, 
# 利用colClasses設定每欄資料的class，可以加快讀取速度
  colClasses = c("factor", "integer", "integer", "factor", "factor", 
    "numeric", "numeric", "integer", "numeric", "integer", "integer", 
    "numeric", "numeric", "integer", "integer", "numeric", "numeric", 
    "numeric", "numeric", "numeric", "numeric"))

# read.csv可以直接讀取csv檔，自動以逗號分隔，並以第一列為欄位名稱
ubike <- read.csv(path)

```

--- &twocol
## 取值
*** =left

```r
x <- c(4.39, 2.11, 3.17)
x[c(1, 3)]  # 選擇第1和3個元素 
```

```
## [1] 4.39 3.17
```

```r
x[-1]  # 移除第1個元素
```

```
## [1] 2.11 3.17
```


*** =right

```r
x > 3  # 判斷x中的元素是否大於3
```

```
## [1]  TRUE FALSE  TRUE
```

```r
which(x > 3)  # x中哪個元素大於3
```

```
## [1] 1 3
```

```r
x[which(x > 3)]  # 選出x中大於3的元素
```

```
## [1] 4.39 3.17
```

```r
x[x > 3]  # simplify expression
```

```
## [1] 4.39 3.17
```


---
## data.frame 物件簡介
資料表 (data.frame) 是向量 (vector) 的一種推廣，它可以將多個相同長度 (不一定是相同類別) 的向量合併在一起 (combine by column)。

```r
x <- c(4.39, 2.11, 3.17)
y <- c("apple", "book", "cat")
z <- c(TRUE, FALSE, TRUE)
df <- data.frame(v1 = x, v2 = y, v3 = z)
df
```

```
##     v1    v2    v3
## 1 4.39 apple  TRUE
## 2 2.11  book FALSE
## 3 3.17   cat  TRUE
```

---
## data.frame 物件簡介


```r
str(df)  # 展示物件各欄位的屬性結構
```

```
## 'data.frame':	3 obs. of  3 variables:
##  $ v1: num  4.39 2.11 3.17
##  $ v2: Factor w/ 3 levels "apple","book",..: 1 2 3
##  $ v3: logi  TRUE FALSE TRUE
```

```r
colnames(df)  # 展示物件的欄位名稱
```

```
## [1] "v1" "v2" "v3"
```

```r
rownames(df)  # 展示物件的列名稱
```

```
## [1] "1" "2" "3"
```


--- &twocol
## data.frame的取值
利用[,] 提取物件內容，基本表達式為x[i, j]，表示x物件中第i列 (ith row)、第j行 (jth column) 的值，也可用x[i, ]表達第i列的向量；x[,j]表達第j行的向量。
*** =left

```r
df[1]  # 選擇第一欄
```

```
##     v1
## 1 4.39
## 2 2.11
## 3 3.17
```

```r
df[, 1]  # 選擇第一欄的數值
```

```
## [1] 4.39 2.11 3.17
```

*** =right

```r
df["v1"]  # 選擇第一欄
```

```
##     v1
## 1 4.39
## 2 2.11
## 3 3.17
```


```r
df$v1  # 選擇第一欄的數值
df[["v1"]]
```


```
## [1] 4.39 2.11 3.17
```


---
## 練習取值

請取出df第三列，第二行的數值

    x <- c(4.39, 2.11, 3.17)
    y <- c("apple", "book", "cat")
    z <- c(TRUE, FALSE, TRUE)
    df <- data.frame(v1 = x, v2 = y, v3 = z,stringsAsFactors=FALSE)

1. _df[3,2]_
2. _df$v2[3]_
3. df[["v3"]][2]

*** .hint


*** .explanation
df[3,2]
df$v2[3]
df[["v2"]][3]

---
## data.frame的取值
可以用["欄位名稱"]，選擇特定欄位，也可以用 $  來提取物件的特定欄位，請試著在 df$ 之後按tab (自動完成鍵)。中括號中可以使用條件算子進行取值。

```r
df[2, ]  # select 2nd row
```

```
##     v1   v2    v3
## 2 2.11 book FALSE
```

```r
df[df$v1 > 3 & z == TRUE, "v2"]
```

```
## [1] apple cat  
## Levels: apple book cat
```


---
## data.frame的合併
利用rbind (上下合併)、cbind (左右合併) 對data.frame進行合併

```r
x <- data.frame(Drama = c("我的自由年代", "回到愛以前"), TV = c("三立", 
    "台視"))

y <- data.frame(Vol = c(12, 13), Rating = c(2.67, 2.58))

z <- data.frame(Drama = c("16個夏天", "妹妹"), TV = c("公視", "台視"), 
    Vol = c(16, 7), Rating = c(2.3, 1.3))
```

--- &twocol
## data.frame的合併
*** =left

```r
x
```

```
##          Drama   TV
## 1 我的自由年代 三立
## 2   回到愛以前 台視
```

```r
y
```

```
##   Vol Rating
## 1  12   2.67
## 2  13   2.58
```

*** =right

```r
xy <- cbind(x, y)
xy
```

```
##          Drama   TV Vol Rating
## 1 我的自由年代 三立  12   2.67
## 2   回到愛以前 台視  13   2.58
```

---
## data.frame的合併

```r
z
```

```
##      Drama   TV Vol Rating
## 1 16個夏天 公視  16    2.3
## 2     妹妹 台視   7    1.3
```

```r
rbind(xy, z)
```

```
##          Drama   TV Vol Rating
## 1 我的自由年代 三立  12   2.67
## 2   回到愛以前 台視  13   2.58
## 3     16個夏天 公視  16   2.30
## 4         妹妹 台視   7   1.30
```

```r
# 壓縮程式碼 rbind(cbind(x, y),z)
```


--- &twocol
## Arithmetic Operator
*** =left



```r
x + y  # x=50;y=3
```

```
## [1] 53
```

```r
x - y
```

```
## [1] 47
```

```r
x * y
```

```
## [1] 150
```

```r
x^y  # x的y次方
```

```
## [1] 125000
```

*** =right

```r
x/y  # x除以y
```

```
## [1] 16.67
```

```r
x%%y  # x除以y的餘數
```

```
## [1] 2
```

```r
x%/%y  # x除以y的商數
```

```
## [1] 16
```


---
## Logical Operator

| Operator  | Description | 
|-----------|-------|
|   <   |   小於   |
|   <=   |  不大於    |
|   >   |   大於   |
|   >=   |  不小於    |
|   ==   |  兩者相等  |
|   !=   |  不等於  |
|   !x   |  非x  |


### 多重條件

- 且：`&`
    - `布林運算結果1 & 布林運算結果1`
- 或：`|`
    - `布林運算結果1 | 布林運算結果1`




--- &twocol .largecontent
## formula
*** =left
<br>
<br>
<br>
formula相當於對模型的描述，基本的表示方法為y~x，x和y分別為不同的向量變數，以~隔開，而y~x可想成**觀察變數y相對於變數x的變化**。例如：觀察濕度相對於降雨量的變化。

*** =right

```r
plot(humidity ~ rainfall, ubike)
```

![plot of chunk unnamed-chunk-29](figure/unnamed-chunk-29.png) 


--- &vcenter .largecontent
## 練習Formula

觀察氣壓相對於溫度的變化。

![plot of chunk unnamed-chunk-30](figure/unnamed-chunk-30.png) 


--- &vcenter .largecontent
## 練習Formula-Answer

```r
plot(pressure ~ temp, ubike)
```

![plot of chunk unnamed-chunk-31](figure/unnamed-chunk-31.png) 


--- &vcenter .largecontent
## formula

有時待觀察的變數可能被不只一個變數影響，而這些變數之間也有可能會相互影響，模型的描述就會比較複雜


    y~x+w+z     # 觀察y相對於x, w, z的變化
    y~x*z       # 觀察y相對於x, z, xz的變化
    y~x:z       # 觀察y相對於xz的變化
    y~(x+z)^2   # 觀察y相對於x, z, xz的變化
    y~x:z-x     # 觀察y相對於z, xz的變化



--- &radio

## 練習Formula

在ubike中，想建立humidity與temp、pressure和rainfall之間的模型，formula該如何表示？

1. humidity~temp:pressure+rainfall
2. _humidity~temp+pressure+rainfall_
3. humidity~temp+pressure *rainfall

*** .hint
請愛用+

*** .explanation
1. temp:pressure、rainfall
2. temp、pressure和rainfall
3. temp、pressure、rainfall和pressure:rainfall

--- .dark .segue

## Exploratory Data Analysis

--- &vcenter .largecontent

## EDA的複習

- 了解如何看數據
    - 類別型數據
    - 數值型數據
    - 單一數據
    - 兩欄數據

---
## 列表觀察類別型數據
<!-- html table generated in R 2.15.3 by xtable 1.7-1 package -->
<!-- Sat May 16 20:41:56 2015 -->
<TABLE border=1>
<TR> <TH>  </TH> <TH> 房價 </TH> <TH> 信義區 </TH> <TH> 大安區 </TH>  </TR>
  <TR> <TD align="right"> 1 </TD> <TD> 25% </TD> <TD align="right"> 12.50 </TD> <TD align="right"> 14.80 </TD> </TR>
  <TR> <TD align="right"> 2 </TD> <TD> 50% </TD> <TD align="right"> 18.00 </TD> <TD align="right"> 23.40 </TD> </TR>
  <TR> <TD align="right"> 3 </TD> <TD> 75% </TD> <TD align="right"> 25.60 </TD> <TD align="right"> 37.40 </TD> </TR>
  <TR> <TD align="right"> 4 </TD> <TD> 平均 </TD> <TD align="right"> 25.20 </TD> <TD align="right"> 31.50 </TD> </TR>
   </TABLE>

--- &vcenter
## Barplot觀察類別與數值
![plot of chunk unnamed-chunk-33](figure/unnamed-chunk-33.png) 


--- &vcenter
## Boxplot觀察不同類別數據分布
![plot of chunk unnamed-chunk-34](figure/unnamed-chunk-34.png) 


--- &vcenter
## Density plot觀察單一數據的分布情形
![plot of chunk unnamed-chunk-35](figure/unnamed-chunk-35.png) 



--- &twocol .largecontent

## Scattor plot觀察兩種數據的分布

*** =left

![plot of chunk cars1](figure/cars1.png) 

*** =right
![plot of chunk unnamed-chunk-36](figure/unnamed-chunk-36.png) 


--- &vcenter
## 觀察時間與數據之間的關係

![plot of chunk unnamed-chunk-37](figure/unnamed-chunk-37.png) 


--- .largecontent
## 總結
- 學Linear Model前的準備
    - R Basic
        - 賦值與取值
        - 讀檔
        - factor
        - formula
    - Exploratory Data Analysis
        - 敘述統計量與視覺化
