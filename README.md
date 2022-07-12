## 리얼페이 소개

리얼페이는 FiveM 에서 사용자로 부터 암호화폐(코인) 지불을 받을 수 있도록 제작된 결제 시스템입니다.
<br>

리얼페이 API를 이용하여 결제시스템을 귀하의 운영중인 FiveM 서버와 손쉽게 통합할 수 있으며,
<br>

지불받은 코인의 충전 또는 구매 등의 처리를 현재 코인시세에 따라 FiveM 서버에서 자동으로 수행할 수 있습니다.
<br>

리얼페이 머천트 관리 페이지에서는 사용자가 결제한 코인 및 내역을 관리할 수 있습니다.
<br><br>

- 머천트 페이지: <https://pay.realw.kr>
<br><br>

## API 엔드포인트

- 메인 노드

```
https://api.realw.kr/cp/v1
```

### 엔드포인트에 대한 일반 정보

- `GET` 엔드포인트의 경우 매개변수를 `query string` 으로 전송할 수 있습니다.
- `POST`, `PUT` 및 `DELETE` 엔드포인트의 경우 `content-type` 이 `application/json` 요청으로 전송될 수 있습니다.
- 매개변수는 어떤 순서로든 보낼 수 있습니다.
- `query string` 과 `request body` 모두에 매개변수가 전송되면 `query string` 매개변수가 사용됩니다.

### HTTP 반환 코드

- HTTP 4XX 반환 코드는 잘못된 요청에 사용됩니다. 문제는 호출자 측에 있습니다.
- HTTP 403 반환 코드는 WAF 제한(웹 응용 프로그램 방화벽)을 위반했을 때 사용됩니다.
- HTTP 429 반환 코드는 요청 속도 제한을 위반할 때 사용됩니다.
- HTTP 418 반환 코드는 코드를 받은 후 요청을 계속 보내기 위해 IP가 자동 금지되었을 때 사용됩니다.
- HTTP 5XX 반환 코드는 내부 오류에 사용됩니다. 문제는 API 서버측입니다.

### 엔드포인트 인증

API 호출에는 머천트 페이지에서 발급받은 API KEY 를 요구 합니다. API KEY 는 API 호출 요청시 `X-API-Credential` 헤더로 전송합니다.

- HTTP Header:

```
X-API-Credential: API Key
```

<br>

## 지원하는 코인 및 토큰

### 지원하는 코인

- `BTC`&nbsp;&nbsp;&nbsp;Bitcoin
- `ETH`&nbsp;&nbsp;&nbsp;Ethereum
- `TRX`&nbsp;&nbsp;&nbsp;Tron
- `XRP`&nbsp;&nbsp;&nbsp;Ripple
- `ETC`&nbsp;&nbsp;&nbsp;Ethereum Classic
- `BCH`&nbsp;&nbsp;&nbsp;Bitcoin Cash
- `DASH`&nbsp;Dash
- `LTC`&nbsp;&nbsp;&nbsp;Litecoin
- `DOGE`&nbsp;Dogecoin
- `ZEC`&nbsp;&nbsp;&nbsp;Zcash

### 지원하는 토큰

- `USDT`&nbsp;Tether
- `USDC`&nbsp;USD Coin
- `DAI`&nbsp;&nbsp;&nbsp;Dai
<br>

## API 목록

### API 상태

- 호출

```
GET /system/status
```

- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/system/status \
     --header 'X-API-Credential: Your API Key'
```

- 응답 예시

```json
{
  "status": 0,
  "msg": "normal",
}
```

<br>

### 이용가능 코인 및 환율 정보

- 호출

```
GET /asset/list
```

- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/asset/list \
     --header 'X-API-Credential: Your API Key'
```

- 응답 예시

```json
{
  "data": {
    "USDT": {
      "rates": {
        "KRW": "1274",
        "USD": "1.01",
        "EUR": "0.95"
      },
      "name": "테더",
      "confirms": 3
    },
    "TRX": {
      "rates": {
        "KRW": "103",
        "USD": "0.08",
        "EUR": "0.08"
      },
      "name": "트론",
      "confirms": 2
    },
    "XRP": {
      "rates": {
        "KRW": "504",
        "USD": "0.40",
        "EUR": "0.38"
      },
      "name": "리플",
      "confirms": 2
    }
  }
}

```

<br>

### 머천트 정보

- 호출

```
GET /account/info
```

- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/account/info \
     --header 'X-API-Credential: Your API Key'
```

- 응답 예시

```json
{
  "data": {
    "merchantId": "f100bc8b-e0d7-21ec-a49d-2220cde77bfd",
    "email": "test@realw.kr",
    "accountName": "",
    "localCurrency": "KRW",
    "businessName": "RealPay",
    "businessUrl": "https://www.realw.kr",
    "supportEmail": "support@realw.kr",
    "createdAt": "2022-05-01T11:50:59.000Z"
  }
}
```

<br>

### 머천트 잔고

- 호출

```
GET /account/balances
```

- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/account/balances \
     --header 'X-API-Credential: Your API Key'
```

- 응답 예시

```json
{
  "data": {
    "USDT": {
      "balance": "0.000000000000000000"
    },
    "TRX": {
      "balance": "0.000000"
    },
    "XRP": {
      "balance": "0.000000"
    },
    "ETH": {
      "balance": "0.000000000000000000"
    },
    "BTC": {
      "balance": "0.00000000"
    }
  }
}
```

<br>

### 입금 주소 생성

- 호출

```
GET /payment/getAddress/:currency/:id
```

- 경로 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제창ID 또는 결제창CODE</td>
    </tr>
    <tr>
      <td>currency</td>
      <td>STRING</td>
      <td>예</td>
      <td>암호화폐ID (예시: BTC, ETH)</td>
    </tr>
  </tbody>
</table>


- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/payment/getAddress/BTC/ad74de9e-e2cc-21ec-349d-f120cde77bfd
```

- 응답 예시

```json
{
  "data": {
    "paymentId": "ad74de9e-e2cc-21ec-349d-f120cde77bfd",
    "address": "3A1fL8pKM1YL9LqcXPTYmPAieuDB4CGTcA",
    "currency": "BTC",
  }
}
```

<br>

### 결제창 생성

- 호출

```
POST /payment/new
```

- 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>name</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제창 이름</td>
    </tr>
    <tr>
      <td>description</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제창 설명</td>
    </tr>
    <tr>
      <td>pricingType</td>
      <td>STRING</td>
      <td>예</td>
      <td>fixed</td>
    </tr>
    <tr>
      <td>localPrice</td>
      <td>OBJECT</td>
      <td>예</td>
      <td>amount:결제받을 금액<br>currency:결제받을 법정통화 (예시: USD,EUR,KRW)</td>
    </tr>
    <tr>
      <td>redirectUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td>결제 완료 후 리다이렉션 될 페이지 URL</td>
    </tr>
    <tr>
      <td>cancelUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td>결제 취소 후 리다이렉션 될 페이지 URL</td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>아니요</td>
      <td>참조할 커스텀 데이터 (웹훅 전송시 해당 값이 전송됩니다.)</td>
    </tr>
  </tbody>
</table>

- 요청 예시

```
curl --request POST \
     --url https://api.realw.kr/cp/v1/payment/new \
     --header 'Content-Type: application/json' \
     --header 'X-API-Credential: Your API Key' \
     --data '
      {
        "name": "Payment Test",
        "description": "Test Description",
        "pricingType": "fixed",
        "localPrice": {
          "amount": "1000000",
          "currency": "KRW"
        },
        "metadata": {
          "user_id": "2293382",
          "order_id": "339128"
        }
      }
     '
```

- 응답 예시

```json
{
  "data": {
    "id": "a01549c5-e714-11ec-b492-f320cde77bfd",
    "code": "A119A12V",
    "name": "Payment Test",
    "description": "Test Description",
    "hostedUrl": "https://pay.realw.kr/payment/A119A12V",
    "createdAt": "2022-06-01T10:20:58Z",
    "expiresAt": "2022-06-01T11:20:58Z",
    "pricingType": "fixed",
    "pricing": {
      "BCH": "4.412751",
      "BTC": "0.02593876",
      "DAI": "784.93",
      "ETC": "37.131930",
      "ETH": "0.438665",
      "LTC": "12.692288",
      "TRX": "9708.737864",
      "XRP": "1984.126984",
      "ZEC": "8.662659",
      "DASH": "13.516436",
      "DOGE": "9803.921569",
      "USDC": "784.93",
      "USDT": "784.93"
    },
    "payCurrency": "BTC",
    "payAmount": "0",
    "payPendingAmount": "0",
    "dueAmount": "0",
    "localAmount": "1000000",
    "localCurrency": "KRW",
    "payCurrency": null,
    "exchangeRates": {
      "BCH": "226616",
      "BTC": "38552348",
      "DAI": "1274",
      "ETC": "26931",
      "ETH": "2279643",
      "LTC": "78788",
      "TRX": "103",
      "XRP": "504",
      "ZEC": "115438",
      "DASH": "73984",
      "DOGE": "102",
      "USDC": "1274",
      "USDT": "1274"
    },
    "timeline": [],
    "status": "NEW",
    "expireTimeLeft": 0
  }
}
```

- 결제창 생성후 웹 결제 페이지를 사용자에게 제공하고 지불을 받을 수 있습니다.

```
https://pay.realw.kr/payment/:id
```

경로 매개변수 id 는 `결제ID` 또는 `결제CODE` 로 사용됩니다.

- 예시

```
https://pay.realw.kr/payment/AJ3SOZV9
```

<br>

### 결제창 취소

- 호출

```
POST /payment/cancel/:id
```

- 경로 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제창ID 또는 결제창CODE</td>
    </tr>
  </tbody>
</table>

- 요청 예시

```
curl --request POST \
     --url https://api.realw.kr/cp/v1/payment/cancel/a01549c5-e714-11ec-b492-f320cde77bfd \
     --header 'Content-Type: application/json' \
     --header 'X-API-Credential: Your API Key'
```

<br>

### 결제창 정보

- 호출

```
GET /payment/get/:id
```

- 경로 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>결제창ID 또는 결제창CODE</td>
    </tr>
  </tbody>
</table>

- 요청 예시

```
curl --request GET \
     --url https://api.realw.kr/cp/v1/payment/get/a01549c5-e714-11ec-b492-f320cde77bfd
```

<br>

### 출금 생성

- 호출

```
POST /withdraw/new
```

- 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>currency</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금할 암호화폐ID (예시: BTC, ETH)</td>
    </tr>
    <tr>
      <td>amount</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금할 금액</td>
    </tr>
    <tr>
      <td>address</td>
      <td>STRING</td>
      <td>예</td>
      <td>받을 지갑주소</td>
    </tr>
    <tr>
      <td>addressTag</td>
      <td>STRING</td>
      <td>아니요</td>
      <td>받을 지갑주소 태그</td>
    </tr>
    <tr>
      <td>autoConfirm</td>
      <td>BOOLEAN</td>
      <td>아니요</td>
      <td>출금 자동 확인 여부를 설정합니다. 값이 false 경우 머천트 페이지 인출 페이지에서 수동으로 관리자가 확인 처리 후 코인이 전송됩니다. (기본값:false)</td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>아니요</td>
      <td>사용자 커스텀 데이터</td>
    </tr>
  </tbody>
</table>
<br>

### 출금 취소

- 호출

```
POST /withdraw/cancel/:id
```

- 경로 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금ID</td>
    </tr>
  </tbody>
</table>
<br>

### 출금 정보

- 호출

```
GET /withdraw/get/:id
```

- 경로 매개변수

<table>
  <thead>
    <tr>
      <td>매개변수 명</td>
      <td>매개변수 타입</td>
      <td>필수값</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>예</td>
      <td>출금ID</td>
    </tr>
  </tbody>
</table>

<br>

## 이벤트 웹훅

이벤트 웹훅은 결제창의 상태가 변경될 경우 지정한 웹훅 주소로 상태 정보를 전송합니다.

머천트는 해당 데이터를 받아 사용자가 입금한 내역을 확인하고 내부적으로 거래를 처리할 수 있습니다.

### 웹훅 데이터 보안

전송받은 웹훅 데이터가 스팸에 의한 것이 아닌 올바른 데이터 여부를 확인합니다.

모든 웹훅 요청에는 원시 데이터에 대한 `HMAC SHA256` 값이 헤더에 포함됩니다.

해당 값은 `X-Webhook-Signature` 헤더로 전송됩니다.

`X-Webhook-Signature: HMAC SHA256 Hash`

해당 서명은 머천트 페이지의 `Webhook Secret Key` 를 키를 이용하여 암호화 됩니다.

머천트는 해당 헤더를 비교하여 올바르지 않을 경우 웹훅 데이터에 대한 처리를 거부해야 합니다.
<br>

### 웹훅 데이터 예시

모든 웹훅 데이턴는 `HTTP POST` 방식의 `application/json` 로 전송됩니다.

```json
{
  "id": "ac855633-ab15-13ec-a49d-f21acde77bfd",
  "resource": "event",
  "type": "payment:created",
  "createdAt": "2022-05-23T18:57:46.000Z",
  "data": {
    "id": "bb6d9079-ab11-13ac-249d-2w20cde77bfd",
    "code": "NUMD1YYA",
    "name": "Payment",
    "description": "Payment Test",
    "status": "NEW",
    "pricingType": "fixed",
    "pricing": {},
    "metadata": {},
    "createdAt": "2022-05-23T18:57:45.000Z",
    "expiresAt": "2022-05-23T19:57:45.000Z",
    "localAmount": "10000.000000000000000000000000",
    "localCurrency": "KRW",
    "payCurrency": "ETH",
    "payAmount": "0.000000"
  }
}
```

- 웹훅 헤더 필드

<table>
  <thead>
    <tr>
      <td>필드 명</td>
      <td>필드 타입</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>웹훅ID</td>
    </tr>
    <tr>
      <td>resource</td>
      <td>STRING</td>
      <td>event: 웹훅 이벤트</td>
    </tr>
    <tr>
      <td>type</td>
      <td>STRING</td>
      <td>payment:created: 신규 결제창 생성시<br>payment:pending: 금액이 입금되었을시<br>payment:confirmed: 금액 입금이 확인되어 결제 완료시<br>payment:failed: 결제 실패 또는 취소시</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>STRING</td>
      <td>웹훅 생성 시간</td>
    </tr>
  </tbody>
</table>

- 웹훅 데이터 필드

<table>
  <thead>
    <tr>
      <td>필드 명</td>
      <td>필드 타입</td>
      <td>설명</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>id</td>
      <td>STRING</td>
      <td>웹훅 ID</td>
    </tr>
    <tr>
      <td>code</td>
      <td>STRING</td>
      <td>웹훅 CODE</td>
    </tr>
    <tr>
      <td>name</td>
      <td>STRING</td>
      <td>결제창 이름</td>
    </tr>
    <tr>
      <td>description</td>
      <td>STRING</td>
      <td>결제창 설명</td>
    </tr>
    <tr>
      <td>status</td>
      <td>STRING</td>
      <td>결제창 상태</td>
    </tr>
    <tr>
      <td>pricing</td>
      <td>OBJECT</td>
      <td>{"BTC":"0.02293","ETH":"0.2193"}</td>
    </tr>
    <tr>
      <td>incoming</td>
      <td>OBJECT</td>
      <td>{"BTC":"0.00293"}</td>
    </tr>
    <tr>
      <td>metadata</td>
      <td>OBJECT</td>
      <td>커스텀 필드</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>STRING</td>
      <td>결제창 생성시간</td>
    </tr>
    <tr>
      <td>expiresAt</td>
      <td>STRING</td>
      <td>결제창 만료시간</td>
    </tr>
    <tr>
      <td>localAmount</td>
      <td>STRING</td>
      <td>결제 금액</td>
    </tr>
    <tr>
      <td>localCurrency</td>
      <td>STRING</td>
      <td>결제 금액의 통화</td>
    </tr>
    <tr>
      <td>payCurrency</td>
      <td>STRING</td>
      <td>결제한 통화 (코인)</td>
    </tr>
    <tr>
      <td>payAmount</td>
      <td>STRING</td>
      <td>결제한 금액 (코인)</td>
    </tr>
    <tr>
      <td>pricingType</td>
      <td>STRING</td>
      <td>fixed</td>
    </tr>
  </tbody>
</table>