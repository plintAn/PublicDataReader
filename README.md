# PublicDataReader

#PublicDtaReader 라이브러리는 공공 데이터 조회를 위한 오픈소스 파이썬 라이브러리이다. 이를 활용하기 위해서는 회원가입 후 API 활용 신청을 하고 개인 키를 받아 사용하면 된다.

## [국토교통부_아파트매매 실거래 상세 자료](https://www.data.go.kr/data/15057511/openapi.do)
 자료소개: 부동산 거래신고에 관한 법률에 따라 신고된 주택의 실거래 자료를 제공

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

### 마포구 아파트 조회
## 기간 : 2023년 01월 ~ 2023년 06월
json 파일로 저장.


```bash
export SERVICE_KEY=your_personal_key_here
```

```python
from PublicDataReader import TransactionPrice
import PublicDataReader as pdr

# 서비스 키 등록
service_key = "YOURKEY"
api = TransactionPrice(service_key)

# 검색할 지역 이름 설정
sigungu_name = "마포구"
code = pdr.code_bdong()
sigungu_code = code.loc[(code['시군구명'].str.contains(sigungu_name)) &
                        (code['읍면동명'] == ''), '시군구코드'].values[0]

# 특정 기간 동안 해당 지역 아파트 거래 조회
df = api.get_data(
    property_type="아파트",
    trade_type="매매",
    sigungu_code=sigungu_code,
    start_year_month="202301",
    end_year_month="202307",  # 종료 월을 7월로 업데이트
)

```


결과 출력


```python
# 결과 출력
import pandas as pd

# DataFrame을 CSV 파일로 CP949 인코딩으로 저장
df.to_csv('apartment_transactions.csv', index=False, encoding='cp949')

# 저장된 CSV 파일을 데이터프레임으로 다시 불러오기 (선택사항)
loaded_df = pd.read_csv('apartment_transactions.csv', encoding='cp949')

# 불러온 데이터프레임 출력 (선택사항)
print(loaded_df)
```
![image](https://github.com/plintAn/PublicDataReader/assets/124107186/9dc0fa82-37a5-45c8-a7a6-943dee9dbfde)












