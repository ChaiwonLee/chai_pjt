---
title: "1973년 미국 강력범죄에 대한 주성분 분석"
author: "이채원"
date: "2023-12-19"
output:
  html_document:
    keep_md: true
---
---
#### **0. 데이터 개요**  
- 1973년 미국 각 주 강력범죄 데이터  
- 변수 Murder, Assault, Rape는 인구 100,000명당 사고 건 수  
- 변수 UrbanPop은 도시인구 비율   
<br/>

#### **1. 데이터 불러오기**   

```r
uscrime <- read.csv('https://raw.githubusercontent.com/ChaiwonLee/chai_pjt/main/2023/uscrime/uscrime.csv?token=GHSAT0AAAAAACLY6AY3RFGQYB4QEYCIWNHMZMCJTOQ',fileEncoding = "utf-8")
```
<br/>

#### **2. 데이터 확인하기**

```r
head(uscrime)
```

```
##        state Murder Assault UrbanPop Rape
## 1    Alabama   13.2     236       58 21.2
## 2     Alaska   10.0     263       48 44.5
## 3    Arizona    8.1     294       80 31.0
## 4   Arkansas    8.8     190       50 19.5
## 5 California    9.0     276       91 40.6
## 6   Colorado    7.9     204       78 38.7
```
- Alabama주의 경우 인구 100,000명당 13.2건의 살인사건(Murder) 발생, 236건의 폭행사건(Assault) 발생, 21.2건의 강간사건(Rape) 발생
- 각 주별로 데이터를 통해 위와 같이 확인할 수 있음  
<br/>

#### **3. 데이터 구조 살펴보기**

```r
str(uscrime)
```

```
## 'data.frame':	50 obs. of  5 variables:
##  $ state   : chr  "Alabama" "Alaska" "Arizona" "Arkansas" ...
##  $ Murder  : num  13.2 10 8.1 8.8 9 7.9 3.3 5.9 15.4 17.4 ...
##  $ Assault : int  236 263 294 190 276 204 110 238 335 211 ...
##  $ UrbanPop: int  58 48 80 50 91 78 77 72 80 60 ...
##  $ Rape    : num  21.2 44.5 31 19.5 40.6 38.7 11.1 15.8 31.9 25.8 ...
```
- 주 이름(state)는 문자형, 나머지(Murder, Assault, Rape, UrbanPop)는 정수, 실수형이므로 별도의 변수 가공이 필요하지 않음  
<br/>

#### **4. 분석 전 가독성이 좋게 하기 위해 열 위치를 바꿈**
- 도시인구비율(UrbanPop)이 주 이름(state)다음으로 오게 함

```r
uscrime <- uscrime[,c(1,4,2,3,5)]
head(uscrime)
```

```
##        state UrbanPop Murder Assault Rape
## 1    Alabama       58   13.2     236 21.2
## 2     Alaska       48   10.0     263 44.5
## 3    Arizona       80    8.1     294 31.0
## 4   Arkansas       50    8.8     190 19.5
## 5 California       91    9.0     276 40.6
## 6   Colorado       78    7.9     204 38.7
```
<br/>

#### **5. 요약통계량**

```r
summary(uscrime)
```

```
##     state              UrbanPop         Murder          Assault     
##  Length:50          Min.   :32.00   Min.   : 0.800   Min.   : 45.0  
##  Class :character   1st Qu.:54.50   1st Qu.: 4.075   1st Qu.:109.0  
##  Mode  :character   Median :66.00   Median : 7.250   Median :159.0  
##                     Mean   :65.54   Mean   : 7.788   Mean   :170.8  
##                     3rd Qu.:77.75   3rd Qu.:11.250   3rd Qu.:249.0  
##                     Max.   :91.00   Max.   :17.400   Max.   :337.0  
##       Rape      
##  Min.   : 7.30  
##  1st Qu.:15.07  
##  Median :20.10  
##  Mean   :21.23  
##  3rd Qu.:26.18  
##  Max.   :46.00
```
- 결측치가 존재하지 않으므로 그대로 분석을 진행함  
<br/>

#### **6. 각 주별, 각 강력범죄 별 가장 적은/가장 많은 값들을 찾기**
#### **6-1. 도시인구 비율이 가장 큰 주/작은 주의 각 강력범죄 사고 건 수**

```r
uscrime[which.max(uscrime$UrbanPop),] 
```

```
##        state UrbanPop Murder Assault Rape
## 5 California       91      9     276 40.6
```

```r
uscrime[which.min(uscrime$UrbanPop),] 
```

```
##      state UrbanPop Murder Assault Rape
## 45 Vermont       32    2.2      48 11.2
```
- ***도시인구비율(UrbanPop)***이 가장 ***큰 주(state)***는 91%인 ***California***  
- California의 경우 인구 100,000명 살인사건(Murder)은 9건, 폭행사건(Assault)은 276건, 강간사건(Rape)은 40.6건  
<br/>
- ***도시인구비율(UrbanPop)***이 가장 ***작은 주(state)***는 32%인 ***Vermont***  
- Vermont의 경우 인구 100,000명 당 살인사건(Murder)은 2.2건, 폭행사건(Assault)은 48건, 강간사건(Rape)은 11.2건  
<br/>

#### **6-2. 인구 100,000명 당 살인사건(Murder) 수가 많은/적은 주**

```r
uscrime[which.max(uscrime$Murder),] 
```

```
##      state UrbanPop Murder Assault Rape
## 10 Georgia       60   17.4     211 25.8
```

```r
uscrime[which.min(uscrime$Murder),] 
```

```
##           state UrbanPop Murder Assault Rape
## 34 North Dakota       44    0.8      45  7.3
```
- 인구 100,000명 당 ***살인사건(Murder)*** 수가 ***많은 주*** ***Georgia***
- Georgia의 도시인구비율(UrbanPop)은 60%
- Georgia의 경우 인구 100,000명 당 살인사건(Murder)은 17.4건, 폭행사건(Assault)은 211건, 강간사건(Rape)은 25.8건  
<br/>
- 인구 100,000명 당 ***살인사건(Murder)*** 수가 ***적은 주*** ***North Dakota***
- North Dakota의 도시인구비율(UrbanPop)은 44%
- North Dakota 의 경우 인구 100,000명 당 살인사건(Murder)은 0.8건, 폭행사건(Assault)은 45건, 강간사건(Rape)은 7.3건  
<br/>

#### **6-3. 인구 100,000명 당 폭행사건(Assault) 수가 많은/적은 주**

```r
uscrime[which.max(uscrime$Assault),] 
```

```
##             state UrbanPop Murder Assault Rape
## 33 North Carolina       45     13     337 16.1
```

```r
uscrime[which.min(uscrime$Assault),] 
```

```
##           state UrbanPop Murder Assault Rape
## 34 North Dakota       44    0.8      45  7.3
```
- 인구 100,000명 당 ***폭행사건(Assault)*** 수가 ***많은 주*** ***North Carolina***
- North Carolina의 도시인구비율(UrbanPop)은 45%
- North Carolina의 경우 인구 100,000명 당 살인사건(Murder)은 13건, 폭행사건(Assault)은 337건, 강간사건(Rape)은 16.1건  
<br/>
- 인구 100,000명 당 ***폭행사건(Assault)*** 수가 ***적은 주*** ***North Dakota***
- North Dakota의 도시인구비율(UrbanPop)은 44%
- North Dakota의 경우 인구 100,000명 당 살인사건(Murder)은 0.8건, 폭행사건(Assault)은 45건, 강간사건(Rape)은 7.3건  
<br/>

#### **6-4. 인구 100,000명 당 강간사건(Rape) 수가 많은/적은 주**

```r
uscrime[which.max(uscrime$Rape),] 
```

```
##     state UrbanPop Murder Assault Rape
## 28 Nevada       81   12.2     252   46
```

```r
uscrime[which.min(uscrime$Rape),] 
```

```
##           state UrbanPop Murder Assault Rape
## 34 North Dakota       44    0.8      45  7.3
```
- 인구 100,000명 당 ***강간사건(Rape)*** 수가 ***많은 주*** ***Nevada***
- Nevada의 도시인구비율(UrbanPop)은 81%
- Nevada의 경우 인구 100,000명 당 살인사건(Murder)은 12.2건, 폭행사건(Assault)은 252건, 강간사건(Rape)은 46건  
<br/>
- 인구 100,000명 당 ***강간사건(Rape)*** 수가 ***적은 주*** ***North Dakota***
- North Dakota의 도시인구비율(UrbanPop)은 44%
- North Dakota의 경우 인구 100,000명 당 살인사건(Murder)은 0.8건, 폭행사건(Assault)은 45건, 강간사건(Rape)은 7.3건  
<br/>
- 최대,최소 값을 찾아본 결과 ***North Dakota***의 경우 ***살인사건(Murder), 폭행사건(Assault), 강간사건(Rape)모두 인구 100,000명 발생 건수가 가장 적은 것***으로 나타남  
<br/>

#### **7. 각 강력사건 분포 시각화**

```r
par(mfrow=c(1,3))
hist(uscrime$Murder, main="Murder", xlab=" ", ylab="counts")
hist(uscrime$Assault, main="Assault", xlab=" ", ylab="counts")
hist(uscrime$Rape, main="Rape", xlab=" ", ylab="counts")
```

![](uscrime_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
<hr/>
- 살인사건(Murder)과 강간사건(Rape)의 경우 상대적으로 사건 건수가 적은 주가 많음(왼쪽으로 치우친 종 모양)  
- 폭행사건(Assault)의 경우 살인사건(Murder)이나 강간사건(Rape)에 비해 사건 건수가 어느 정도 발생하는 주가 많음(중앙이 뚱뚱한 종 모양)  
<br/>
- 즉, ***폭행사건(Assault)**은 살인사건(Murder)보다 강간사건(Rape)보다 ***많은 주에서 자주 발생***하는 것이라 추측해볼 수 있음  
<br/>

#### **8. 주성분분석**
- 주 이름은 분석에서 제외하기로 하고 새로운 데이터 uscrime2 생성

```r
uscrime2 <- uscrime[,c(2,3,4,5)]
head(uscrime2)
```

```
##   UrbanPop Murder Assault Rape
## 1       58   13.2     236 21.2
## 2       48   10.0     263 44.5
## 3       80    8.1     294 31.0
## 4       50    8.8     190 19.5
## 5       91    9.0     276 40.6
## 6       78    7.9     204 38.7
```
- 범죄 관련 변수는 사건 발생 건수, 인구 관련 변수는 비율이므로 단위가 다르기 때문에, 주성분분석 시 상관계수 행렬을 사용하기로 함

```r
uscrime2_pca = princomp(uscrime2, cor=T, scores=T)
names(uscrime2_pca)
```

```
## [1] "sdev"     "loadings" "center"   "scale"    "n.obs"    "scores"   "call"
```

```r
uscrime2_pca
```

```
## Call:
## princomp(x = uscrime2, cor = T, scores = T)
## 
## Standard deviations:
##    Comp.1    Comp.2    Comp.3    Comp.4 
## 1.5748783 0.9948694 0.5971291 0.4164494 
## 
##  4  variables and  50 observations.
```

```r
summary(uscrime2_pca)
```

```
## Importance of components:
##                           Comp.1    Comp.2    Comp.3     Comp.4
## Standard deviation     1.5748783 0.9948694 0.5971291 0.41644938
## Proportion of Variance 0.6200604 0.2474413 0.0891408 0.04335752
## Cumulative Proportion  0.6200604 0.8675017 0.9566425 1.00000000
```
- 첫번째 주성분(com.1)이 62%, 두번째 주성분(com.2)이 24.7%의 분산비율을 가지고 있음
- 즉 2개의 주성분이 전체 변동의 86.7%를 설명하고 있음  
<br/>

#### **9. 고유값 계산**
- 주성분 분석의 결과에서 standard deviation를 제곱하여 도출

```r
eig_val = uscrime2_pca$sdev^2
eig_val
```

```
##    Comp.1    Comp.2    Comp.3    Comp.4 
## 2.4802416 0.9897652 0.3565632 0.1734301
```
- 유효한 주성분의 개수는 2개
- comp.2의 경우 소수점 2자리에서 반올림하면 1이 되기 때문에 유효값으로 판단  
<br/>

#### **10. 스크리그림**

```r
screeplot(uscrime2_pca, type="lines", main='scree plot')
```

![](uscrime_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
<br/>

#### **11. 누적분산그림**

```r
uscrime2_var = uscrime2_pca$sdev^2
uscrime2_var_ratio = uscrime2_var/sum(uscrime2_var)
plot(cumsum(uscrime2_var_ratio), type="b", xlab="component", ylab="cumulative proportion")
title("variance explained")
```

![](uscrime_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
<hr/>
- 스크리그림 결과 두번째 주성분(Comp.2) 이후 그래프가 1 밑으로 감소
- 누적분산그림 결과 두번째 주성분 이후 그래프의 기울기가 크게 감소
- 즉, 두번째 주성분까지 보유하는 것이 타당함  
<br/>

#### **12. 주성분계수**

```r
uscrime2_pca$loadings[,c(1:2)]
```

```
##             Comp.1     Comp.2
## UrbanPop 0.2781909  0.8728062
## Murder   0.5358995 -0.4181809
## Assault  0.5831836 -0.1879856
## Rape     0.5434321  0.1673186
```
- pc1 = 0.28UrbanPop + 0.54Murder + 0.58Assault + 0.54Rape
- pc2 = -0.87UrbanPop + 0.42Murder + 0.19Assault + 0.17Rape
- 즉, 미국의 각 주별 인구 100,000명 당 강력범죄를 설명할 때, 위의 두 개의 주성분으로 설명하는 것이 적절하함
<br/>
