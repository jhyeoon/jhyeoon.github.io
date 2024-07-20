---
layout: post
title:  "ML세션 1주차-데이터 분석가가 반드시 알아야할 모든 것 2부 10장"
date:   2024-07-19 20:47:24 +0900
categories: jekyll update
---
# 10.1 탐색적 데이터 분석(EDA)

- Exploratory Data Analysis
- 극단적인 해석은 피하고 지나친 추론이나 자의적 해석 지양해야 함

## EDA의 목적
- 데이터의 형태와 척도가 분석에 알맞게 되어있는지
- 데이터의 평균, 분산 분포, 패턴  등의 확인 → 데이터 특성 파악
- 데이터의 결축값이나 이상치 파악 및 보완
- 변수 간의 관계성 파악
- 분석 목적과 방향성 점검 및 보정

## 엑셀을 활용한 EDA
- 임의로 데이터 추출해서 직접 눈으로 관찰

## 탐색적 데이터 분석 실습
```python
# 필요한 패키지 설치
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
sns.set(color_codes=True)
%matplotlib inline
```

`import seaborn as sns` </br>
seaborn : 데이터 시각화 라이브러리 </br>
as sns : seaborn의 별칭

`import matplotlib.pyplot as plt` </br>
matplotlib : 다양한 종류의 그래프와 플롯을 만들 수 있게 해주는 라이브러리 </br>
pyplot : Matplotlib의 주요 인터페이스, 간단하고 직관적인 방법으로 그래프를 그릴 수 있는 모듈

`import pandas as pd` </br>
pandas : 데이터 조작과 분석을 위해 설계된 라이브러리, 데이터 프레임 구조를 사용

`sns.set(color_codes=True)` </br>
sns.set : Seaborn의 기본 스타일을 설정 </br>
color_codes=True : Seaborn에서 제공하는 기본 색상 팔레트가 Matplotlib의 색상 코드와 일치하도록

`%matplotlib inline` </br>
: Jupyter Notebook에서 Matplotlib 라이브러리를 사용할 때 그래프를 인라인(inline)으로 표시하기 위해 사용되는 매직 명령어 </br>
-Jupyter Notebook의 셀 안에 그래프를 직접 삽입하여, 그래프를 별도의 창이 아닌 셀 안에서 바로 볼 수 있게

</br>

```python
# 데이터 불러오기
# https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand
df = pd.read_csv("datasets/hotel_bookings.csv")

# 데이터 샘플 확인
df.head()
```

`df = pd.read_csv("datasets/hotel_bookings.csv")` </br>
: Pandas 라이브러리를 사용하여 CSV 파일(hotel_bookings.csv)을 데이터 프레임(df)으로 불러오는 명령어

`df.head()` </br>
: 기본적으로 첫 데이터 프레임의 첫 5행을 반환하는 명령어 </br>
-원하는 행 수를 인수로 지정할 수도 있음

</br>

```python
df2 = df[['hotel', 'lead_time']]
df2.head()
```

`df2 = df[['hotel', 'lead_time']]` </br>
: 원본 데이터 프레임 ‘df’에서 'hotel'과 'lead_time' 두 개의 열만 선택하여 새로운 데이터 프레임 ‘df2’를 생성하는 명령어

</br>

```python
# 각 컬럼의 속성 및 결측치 확인
df.info()
```

`df.info()` </br>
: 데이터 프레임의 행 수, 각 열의 데이터 타입, 누락된 값의 개수 등 포함, 데이터 프레임의 구조와 데이터 타입 파악하는 명령어

</br>

```python
# 각 컬럼의 통계치 확인
df.describe()
```

`df.describe()` </br>
: 각 칼럼의 통계치를 파악하는 명령어 </br>
-count: 유효한 값의 개수, mean: 평균값, std: 표준 편차, min: 최솟값, 25%: 1사분위수 (하위 25%), 50%: 중앙값 (하위 50%, 2사분위수), 75%: 3사분위수 (하위 75%), max:최댓값

</br>

```python
# 각 컬럼의 왜도 확인
numeric_df = df.select_dtypes(include=['number'])
skewness = numeric_df.skew()
print(skewness)
```

`df.select_dtypes(include=['number'])` </br>
: df에서 숫자 데이터 타입을 가진 열만 선택

`numeric_df.skew()` </br>
: numeric_df의 각 열의 왜도를 계산하는 명령어 </br>
-왜도: 0에 가까울수록 대칭적인 분포, 양의 값은 오른쪽 꼬리가 긴 분포, 음의 값은 왼쪽 꼬리가 긴 분포

</br>

```python
# 각 컬럼의 첨도 확인
numeric_df = df.select_dtypes(include=['number'])
print(numeric_df.kurtosis())
```

`df.kurtosis()` </br>
: numeric_df의 각 열의 첨도를 계산하는 명령어, 양의 값으로 큰 수면 뾰족한 분포, 음의 값으로 작은 수면 평평한 분포

![alt text](image-1.png)

</br>

```python
# 특정 변수 분포 시각화
plt.rcParams['figure.dpi'] = 300
sns.distplot(df['lead_time'])
```

`plt.rcParams['figure.dpi'] = 300` </br>
: Matplotlib의 설정을 변경하여 생성되는 모든 그래프의 해상도 설정 </br>
-dpi(dots per inch): 그래프의 해상도 결정 </br>
-해상도를 300으로 설정 → 그래프가 더 높은 해상도로 렌더링되어 더 선명하게 보임

`sns.distplot(df['lead_time'])` </br>
: Seaborn을 사용하여 ‘lead_time’ 열의 분포 시각화 </br>
-히스토그램과 커널 밀도 추정(KDE)을 함께 표시

![alt text](image.png)

</br>

```python
# 호텔 구분에 따른 lead_time 분포 차이 시각화
sns.violinplot(x="hotel", y="lead_time", data=df, inner=None, color=".8")
sns.stripplot(x="hotel", y="lead_time", data=df, size=1)
```

`sns.violinplot(x="hotel", y="lead_time", data=df, inner=None, color=".8")` </br>
-sns.violinplot : seaborn의 violinplot </br>
-x="hotel" : 호텔별로 나눔 </br>
-inner=None : violin plot 내부에 추가적인 요소(박스플롯, 개별 데이터 포인트 등)이 없음 </br>
-color=".8" : 회색, 0(흰색), 1(검은색) 

</br>

`sns.stripplot(x="hotel", y="lead_time", data=df, size=1)`
-sns.stripplot : seaborn의 stripplot </br>
-size=1 : 데이터 포인트의 크기가 1 </br>

</br>

![alt text](image-3.png)

# 10.2 공분산과 상관성 분석





