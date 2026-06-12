# 2. 데이터 전처리
## 목차
- [데이터 결측치/이상치 등 처리](#데이터-결측치이상치-처리)
- [데이터 스케일링](#데이터-스케일링)
- [라벨 인코딩/원핫 인코딩](#라벨-인코딩원핫-인코딩)
- [Train/Test 데이터셋 분할](#traintest-데이터셋-분할)

### 데이터 전처리 필요 사례
1. 결측치 처리 <br>
인적사항 중 나이 정보를 누락하였다. 평균 나이 계산 시 오류 발생하므로 해당 값을 삭제하거나 평균값으로 대체하여 사용해야 한다.

2. 이상치 제거 <br>
나이에 `300살`이나 `-10살`과 같은 실제 존재할 수 없는 이상한 데이터 값 존재할 수 있다. 이런 값들은 분석 결과를 왜곡하므로 제거하거나 정말 의미 있는 값이 맞는지 판단해야한다.

3. 데이터 형식 맞추기 <br>
- 온도 데이터가 `°C` or `℉` 
- 날짜 데이터가 `2026-01-01` or `2026/01/01`
- 길이 데이터가 `cm` or `mm` <br>
위 같은 케이스들 처럼 단위가 다른 경우, 하나로 통일 시켜 주어야한다.

4. 스케일링 <br>
스케일링은 각 `변수의 범위를 비슷한 수준으로 맞추는 작업`이다. `나이`와 `연봉`을 예로 들면 연봉의 단위가 더 크기 때문에 스케일링을 하지 않으면 `연봉이 더 중요한 변수로` 판단할 수 있다.

### 데이터 결측치/이상치 처리
- 결측치
```Python
# 1. 각 열 결측치 개수 확인
df.isnull().sum()
```
```python
# 2. 결측치 존재하는 행 삭제
df_clean = df.dropna(axis=0)
```
```python
# 3. 결측치를 평균값으로 채우기 (두 코드 동일한 결과 값)
df.fillna({'채울행':'채울값'}, inplace=True)
```

- 이상치
```python
# 1. 중복여부 확인
df.duplicated()
```
```python
# 2. 중복 행 제거
df.drop_duplicates()
```
```python
# 3. 특정 행/열 삭제
# axis값이 0이면 행(가로)을 1이면 열(세로)을 삭제
df.drop('삭제컬럼명', axis=0 or 1) 
```

- 값 필터링
```python
# 특정 값 포함 데이터 찾기
df[df['컬럼명'].str.isin('검색어')]
# 특정 값 불포함 데이터 찾기
df[~df['컬럼명'].str.isin('검색어')]
```

```python
# 범위지정
df[df['수학'].between(80, 100)] # 수학점수 80~100점인 데이터 가져오기
df[(df['수학'] >= 80) & (df['수학'] <= 100)] #논리 연산
```
```python
# 특정값 제외
df[~df['알파벳'].isin(['A', 'B'])]
```

### 데이터 스케일링
- StandardScaler  <br>
평균을 0, 표준편차를 1로 데이터 재조정 <br>
이상치에 민감하지 않아 많은 ML 알고리즘에서 선택 <br>

```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
train_x_scaled = scaler.fit_transform(train_x)
test_x_scaled = scaler.transform(test_y)
```

- MinMaxScaler <br>
모든 값을 0~1 사이로 압축 <br>
범위가 일정하여 해석하기 쉬우나 이상치에 매우 민감. <br>

```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
train_x_scaled = scaler.fit_transform(train_x)
test_x_scaled = scaler.transform(test_y)
```

- RobustScaler <br>
평균 대신 중앙값, 표준편차 대신 IQR을 사용해 이상치에 강함. <br>
```python
from sklearn.preprocessing import RobustScaler
scaler = RobustScaler()
```

- 이상치 적음 → StandardScaler
- 이상치 많음 → RobustScaler
- 딥러닝 입력값 0~1 필요 → MinMaxScaler


### 라벨 인코딩/원핫 인코딩
- Label Encoding <br>
범주형 카테고리에 숫자를 부여하는 방식. <br>
`['Red', 'Blue', 'Green']` → `[1, 2, 3]`<br>
숫자를 관계의 의미로 받아들여질 수 있어 순서 정보가 있는 범주형 변수에 적합.

```python
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df['Color_Label'] = le.fit_transform(df['Color'])
```

- One-Hot Encoding <br>
카테고리 수 만큼 컬럼을 만들고 해당 값만 1, 나머지는 0 <br>


||RED|BLUE|GREEN|
|-|-|-|-|
|RED|1|0|0|
|BLUE|0|1|0|
|GREEN|0|0|1|

```python
# 1. pandas get_dummies
import pandas as pd
pd.get_dummies(df['Color'], drop_first=True) # True/False
pd.get_dummies(df['Color'], drop_first=True, dtype=int) # 0/1
```
```python
# 2. scikit-learn OneHotEncoder
from sklearn.preprocessing import OneHotEncoder
ohe = OneHotEncoder(drop='fisrt') # drop='first' 다중공선성 방지
encoded = ohe.fit_transform(df[['Color']]) 
```
### Train/Test 데이터셋 분할
```python
from sklearn.model_selection import train_test_split
# 80:20 비율로 데이터 분할
X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size=0.2, random_state=42)
# 분할 데이터셋 확인
print(X_train.shape)
print(X_valid.shape)
print(y_train.shape)
print(y_valid.shape)
```