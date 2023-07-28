## PublicDataReader를 활용한 아파트 매매 실거래가 분석하기

PublicDtaReader 라이브러리는 공공 데이터 조회를 위한 오픈소스 파이썬 라이브러리이다. 이를 활용하기 위해서는 회원가입 후 API 활용 신청을 하고 개인 키를 받아 사용하면 된다.


## [국토교통부_아파트매매 실거래 상세 자료](https://www.data.go.kr/data/15057511/openapi.do)
 자료소개: 부동산 거래신고에 관한 법률에 따라 신고된 주택의 실거래 자료를 제공

## 1. 패키지 설치와 임포트
Python에서는 PublicDataReader 패키지를 사용해 국토교통부_아파트매매 실거래자료 API를 쉽게 이용할 수 있습니다. 만약 아직 설치하지 않으셨다면, 아래의 pip 명령어로 설치가 가능합니다.

<div align="center">

| **서비스명**                          | **부동산 유형** | **거래 유형** |
| ------------------------------------- | ------------ | ------------ |
| [국토교통부_아파트매매 실거래 상세 자료](https://www.data.go.kr/data/15057511/openapi.do)      | 아파트       | 매매         |
| [국토교통부_아파트 전월세 자료](https://www.data.go.kr/data/15058017/openapi.do)               | 아파트       | 전월세       |
| [국토교통부_아파트매매 실거래자료](https://www.data.go.kr/data/15058747/openapi.do)      | 분양입주권   | 매매         |
| [오피스텔 매매 신고 조회](https://www.data.go.kr/data/15058452/openapi.do)               | 오피스텔     | 매매         |
| [오피스텔 전월세 신고 조회](https://www.data.go.kr/data/15059249/openapi.do)             | 오피스텔     | 전월세       |
| [연립다세대 매매 실거래자료 조회](https://www.data.go.kr/data/15058038/openapi.do)       | 연립다세대   | 매매         |
| [연립다세대 전월세 실거래자료 조회](https://www.data.go.kr/data/15058016/openapi.do)     | 연립다세대   | 전월세       |
| [단독/다가구 매매 실거래 조회](https://www.data.go.kr/data/15058022/openapi.do)          | 단독다가구   | 매매         |
| [단독/다가구 전월세 자료 조회](https://www.data.go.kr/data/15058352/openapi.do)          | 단독다가구   | 전월세       |
| [토지 매매 신고 조회](https://www.data.go.kr/data/15056649/openapi.do)                   | 토지         | 매매         |
| [상업업무용 부동산 매매 신고 자료 조회](https://www.data.go.kr/data/15057267/openapi.do) | 상업업무용   | 매매         |
| [공장 및 창고 등 부동산 매매 신고 자료 조회](https://www.data.go.kr/data/15100574/openapi.do) | 공장창고등   | 매매         |

</div>

### 필수 요청 메시지 명세

<div align="center">

| 항목명(영문)   | 항목명(국문)   | 샘플데이터     | 항목설명   |
|:---------------|:---------------|:-------------|:-------------|
| serviceKey     | 인증키         | 인증키(URL Encode) | 인증키  |
| LAWD_CD        | 지역코드       |  11110       | 지역코드     |
| DEAL_YMD       | 계약월         | 201512       | 계약       |

</div>

</div>

### 요청 메세지 명세

| 항목명(영문)    | 항목명(국문)    | 샘플데이터       | 항목설명              |
|---------------|---------------|-----------------|-----------------------|
| resultCode    | 결과코드       | 00              | 결과 코드              |
| resultMsg     | 결과메세지     | NORMAL SERVICE. | 결과 메시지             |
| numOfRows     | 한 페이지 결과 수 | 4              | 한 페이지에서 보여줄 결과 수 |
| pageNo        | 페이지 번호     | 4              | 요청한 페이지 번호       |
| totalCount    | 전체 결과 수     | 4              | 검색된 결과의 총 개수     |
| Deal Amount   | 거래금액        | 82,500         | 거래 금액 (만원)        |
| Build Year    | 건축년도        | 2008           | 건물의 건축년도         |
| Deal Year     | 년             | 2015           | 거래가 계약된 년도       |
| Road Name     | 도로명          | 사직로8길        | 도로명 주소의 도로명 부분  |
| Road Name Bonbun  | 도로명건물본번호코드 | 00004          | 도로명 주소의 건물본번호 부분 |
| Road Name Bubun   | 도로명건물부번호코드 | 00000          | 도로명 주소의 건물부번호 부분 |
| Road Name Sigungu Code | 도로명시군구코드 | 11110          | 도로명 주소의 시군구코드   |
| Road Name Seq   | 도로명일련번호코드   | 03             | 도로명 주소의 일련번호코드 |
| Road Name Basement Code | 도로명지상지하코드 | 0             | 도로명 주소의 지상/지하 코드 |
| Road Name Code   | 도로명코드       | 4100135        | 도로명 주소의 도로명코드   |
| Dong           | 법정동           | 사직동          | 건물이 속한 법정동        |

</div>

```python
pip install PublicDataReader
```
설치 완료 후 임포트

```python
import matplotlib.pyplot as plt
import seaborn as sns
from PublicDataReader import TransactionPrice
import PublicDataReader as pdr
import pandas as pd
```

## 2. 데이터 가져오기
API를 사용하기 위해서는 서비스키가 필요합니다. 서비스키는 공공데이터포털에서 발급받을 수 있습니다.

### 서비스키를 입력
```python
# 서비스키를 입력합니다.
service_key = "your"
api = TransactionPrice(service_key)
```
이번 분석에서는 "성동구"에서 2023년 1월부터 2023년 6월까지의 아파트 매매 실거래가를 조회하겠습니다.

### 마포구 아파트 조회
## 기간 : 2023년 01월 ~ 2023년 06월

```python
# 분석 대상 지역 설정
sigungu_name = "마포구"
code = pdr.code_bdong()
sigungu_code = code.loc[(code['시군구명'].str.contains(sigungu_name)) &
                        (code['읍면동명'] == ''), '시군구코드'].values[0]

# 지정한 기간 (2023년 1월 ~ 6월) 동안의 아파트 거래 데이터 조회
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code=sigungu_code,
    start_year_month="202301",
    end_year_month="202306",  # 6월까지 업데이트
)

```

### 거래년월 컬럼 생성
```python
# '년'과 '월' 컬럼을 이용하여 '거래년월' 컬럼 생성
df['거래년월'] = pd.to_datetime(df['년'] * 10000 + df['월'] * 100 + 1, format='%Y%m%d')
```


## 3. 데이터 처리 및 분석
이제 데이터를 처리하고 분석해보겠습니다. 실거래가 데이터는 어떤 형태로 들어왔는지 확인해보겠습니다.

```python
print(df.head())
```
![image](https://github.com/plintAn/PublicDataReader/assets/124107186/4c70c1fe-8ac7-4783-917e-7bb7d113c3aa)


결과는 다음과 같습니다

## 4. 데이터 시각화
성동구 내의 아파트 매매 실거래가의 트렌드를 확인하기 위해 월별 거래금액의 평균을 계산하고 이를 시각화

```python
# 시각화 설정
sns.set(style="darkgrid", font="Malgun Gothic", font_scale=1.2)
```

결과 출력

```python
# 시각화: 월별 평균 거래금액 추이
plt.figure(figsize=(12, 6))
sns.lineplot(data=df, x='거래년월', y='거래금액', marker='o')
plt.title('마포구 아파트 월별 평균 거래금액', fontsize=16)
plt.xlabel('거래년월', fontsize=14)
plt.ylabel('평균 거래금액(만원)', fontsize=14)
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```
해당 코드는 'PublicDataReader' 라이브러리를 이용하여 서울시 아파트 거래 데이터를 조회하고, 
마포구의 2023년 1월부터 6월까지의 아파트 매매 거래 데이터를 분석

![image](https://github.com/plintAn/PublicDataReader/assets/124107186/7e0501c1-bfc7-4f03-9a99-b8ee2bf8c302)

다음 그래프를 통해 알 수 있는 점

시계열 변동: 그래프 상에서 x축은 시간을 나타내고 있으므로 시계열 데이터로서 변동을 파악할 수 있다.
월별 거래금액 추이: 선의 기울기와 변동을 통해 월별로 아파트 거래금액의 평균이 어떻게 변화하고 있는지 파악할 수 있다.

상승 추세: 그래프 상에서 선이 점차 상승하는 경향을 보이면, 해당 기간 동안 아파트 거래금액이 상승하고 있다는 것을 의미한다.

변동성: 그래프 상에서 선이 교차하거나 진동하는 구간이 있으면, 해당 기간 동안 아파트 거래금액의 변동이 크다는 것을 의미한다.

# 결론 및 시사점

2023년 상반기 마포구 아파트 거래는 비슷한 추이를 보이며, 월별로 큰 변동이 없는 것으로 보여진다.
2023년 상반기 마포구 아파트의 평균 거래금액은 약간의 상승 추세를 보이고 있다.
주기적으로 발생하는 주택 거래 변동을 확인하여 해당 기간 동안의 마포구 아파트 시장 상태를 파악한다.

더 많은 분석과 시각화를 통해 부동산 시장의 동향과 패턴을 파악할 수 있었으며, 이를 통해 투자 및 거래 결정에 도움이 될 수 있을거 같습니다.
추가적인 분석을 통해 더 깊은 인사이트를 얻을 수 있습니다.















