# 1. 데이터 분석
## [참고자료]()
## 목차
- [필요 라이브러리 설치](#필요-라이브러리-설치)
- [데이터 구성 및 특성 파악](#데이터-구성-및-특성-파악)
- [데이터 품질 점검](#데이터-품질-점검)

### 필요 라이브러리 설치
- 라이브러리 설치
```bash
pip install <패키지명>
```

- 라이브러리 불러오기
```python
# 1. import 라이브러리 as 별칭
import pandas as pd
ohe = pd.get_dummies() # 정의한 별칭으로 접근
```
```python
# 2.from 라이브러리 import 특정기능
from sklearn.model_selection import train_test_split
X_train, X_valid, y_train, y_valid = train_test_split(X, y, test_size=0.2, random_state=24)
```

<br>

### 데이터 구성 및 특성 파악

||리스트|튜플|딕셔너리|
|-|-|-|-|
|생성기호|`[]`|`()`|`{}`|
|특징|수정 가능한 가변성|수정 불가능한 불변성 <br> 리스트보다 검색속도가 빠르고 메모리 사용이 적음|수정 가능한 가변성 <br>Key-Value 값으로 접근|
|예시|[1, 2, 3] | (1, 2, 3) | {"영어":[90, 55], "수학":[70, 85]} |



<br>

### 데이터 품질 점검
- 데이터 가져오기 및 데이터 정보 확인

```python
# 1. 데이터 읽어오기 → dataframe 값으로 반환
df_csv = pd.read_csv('파일명.csv')
df_json = pd.read_json('파일명.json')
```

```python
# 2. 데이터 값 확인
df.head() # 상위 n개의 값 출력
df.tail() # 하위 n개의 값 출력
```

```python
# 3.데이터 정보
df.info() # 데이터 결측치, 데이터타입 등 요약
df.describe() # 숫자 데이터의 평균, 최소/최대값 등 기초 통계량 값
```

- 인덱싱
```python
df.iloc[1]
df.iloc[0:3] # 0~2번째 값까지 슬라이싱
```

- 슬라이싱
```python
df.loc[0, '수학']
df.loc[1:2, '수학':'영어']
```