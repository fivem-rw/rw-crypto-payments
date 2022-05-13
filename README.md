```diff
- 해당 API는 테스트 버전 입니다. Production 버전에서 이용하지 마십시오.
- 해당 API는 테스트 단계이므로 API서버에 접속이 원활하지 않을 수 있습니다.
```

## 소개

RW-CryptoPayments API는 RealWorld 에서 제공하는 코인 결제 API로 머천트가 사용자로부터 편리하게 코인 지불을 받을 수 있도록 서비스 및 정보를 제공합니다. API 호출은 표준 HTTP POST (application/x-www-form-urlencoded) 호출로 구현됩니다.
<br><br>
## API 엔드포인트

* 메인 노드
```
https://api.realw.kr/cp/v1
```
* 테스트 노드
```
https://api-test.realw.kr/cp/v1
```

* 공통 매개변수
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
      <td>format</td>
      <td>STRING</td>
      <td>아니요</td>
      <td>반환응답 형식 입니다. json 또는 xml. (기본값: json)</td>
    </tr>
  </tbody>
</table>

### 엔드포인트에 대한 일반 정보
- `GET` 엔드포인트의 경우 매개변수를 `query string` 으로 전송할 수 있습니다.
- `POST`, `PUT` 및 `DELETE` 끝점의 경우 매개변수는 `query string` 으로 전송되거나 `content-type` 이 `application/x-www-form-urlencoded` 요청으로 전송될 수 있습니다.
- 매개변수는 어떤 순서로든 보낼 수 있습니다.
- `query string` 과 `request body` 모두에 매개변수가 전송되면 `query string` 매개변수가 사용됩니다.

### HTTP 반환 코드
- HTTP 4XX 반환 코드는 잘못된 요청에 사용됩니다. 문제는 호출자 측에 있습니다.
- HTTP 403 반환 코드는 WAF 제한(웹 응용 프로그램 방화벽)을 위반했을 때 사용됩니다.
- HTTP 429 반환 코드는 요청 속도 제한을 위반할 때 사용됩니다.
- HTTP 418 반환 코드는 코드를 받은 후 요청을 계속 보내기 위해 IP가 자동 금지되었을 때 사용됩니다.
- HTTP 5XX 반환 코드는 내부 오류에 사용됩니다. 문제는 API 서버측입니다. 이것을 실패 작업으로 취급 하지 않는 것이 중요 합니다. 실행 상태는 UNK아니요WN 이며 성공했을 수 있습니다.

### 엔드포인트 보안

API 호출에는 `Signature Key`로 생성된 `HMAC SHA256` 서명이 포함될 수 있습니다. API 서버에서 자체 HMAC 서명을 생성하고 API 호출자의 서명과 비교합니다. 서명이 일치하지 않으면 API 호출이 대기열에서 삭제됩니다.

- HTTP Header:
```
X-API-Credential: API Key
X-API-Algorithm:  Signature Algorithm
X-API-Signature:  HMAC SHA256
```

> 예시
- API Key: 7b3d979ca8330a94fa7e9e1b466d8b99e0bcdea1ec90596c0dcc8d7ef6b4300c
- Signature Key: 9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08
- 본문 내용: coin=BTC&startTime=1651336421&endTime=1651682021
- 본문 HMAC SHA256: b08ecec4208f1b7b8dabf95e3af744c74e3e3220e64b3b128e50b7385bbccc88
- HTTP Header:
```
X-API-Credential: 7b3d979ca8330a94fa7e9e1b466d8b99e0bcdea1ec90596c0dcc8d7ef6b4300c
X-API-Algorithm:  HMAC-SHA256
X-API-Signature:  b08ecec4208f1b7b8dabf95e3af744c74e3e3220e64b3b128e50b7385bbccc88
```
<br>

## 지원하는 코인 및 토큰

### 코인
- `BTC`&nbsp;&nbsp;&nbsp;Bitcoin
- `ETH`&nbsp;&nbsp;&nbsp;Ethereum
- `XRP`&nbsp;&nbsp;&nbsp;Ripple
- `ETC`&nbsp;&nbsp;&nbsp;Ethereum Classic
- `BCH`&nbsp;&nbsp;&nbsp;Bitcoin Cash
- `DASH`&nbsp;Dash
- `LTC`&nbsp;&nbsp;&nbsp;Litecoin
- `DOGE`&nbsp;Dogecoin
- `ZIL`&nbsp;&nbsp;&nbsp;Zilliqa
- `ZEC`&nbsp;&nbsp;&nbsp;Zcash

### 토큰
- `USDT`&nbsp;Tether
- `LINK`&nbsp;Chainlink
- `USDC`&nbsp;USD Coin
- `UNI`&nbsp;&nbsp;&nbsp;Uniswap
- `WBTC`&nbsp;Wrapped Bitcoin
- `AAVE`&nbsp;Aave
- `SNX`&nbsp;&nbsp;&nbsp;Synthetix
- `DAI`&nbsp;&nbsp;&nbsp;Dai
- `CRO`&nbsp;&nbsp;&nbsp;Cro아니요s
- `MKR`&nbsp;&nbsp;&nbsp;Maker
- `CEL`&nbsp;&nbsp;&nbsp;Celsius
- `COMP`&nbsp;Compound

### 레이어 및 스탠다드
- `ERC-20`
- `ERC-721`
- `Omni Layer`
<br>

## API 목록

* 공통: 모든 API 호출에는 X-API-Credential 헤더가 포함되어야 합니다.
* 서명: 추가적으로 X-API-Algorithm, X-API-Signature 헤더가 포함되어야 합니다.

### API 상태
```
GET /system/status
```

### 이용가능 코인 및 환율 정보
```
GET /asset/coins
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
      <td>short</td>
      <td>BOOLEAN</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 머천트 정보
```
GET /account/info
```

### 머천트 잔고
```
GET /account/balances
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
      <td>all</td>
      <td>BOOLEAN</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 입금 주소 생성
```
POST /deposit/address (서명)
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
      <td>coin</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>callbackUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>label</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 입금 기록
```
GET /deposit/history (서명)
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
      <td>coin</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>status</td>
      <td>INT</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>startTime</td>
      <td>LONG</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>endTime</td>
      <td>LONG</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>limit</td>
      <td>INT</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 출금 신청
```
POST /withdraw/apply (서명)
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
      <td>coin</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>address</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>addressTag</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>amount</td>
      <td>DECIMAL</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>transactionFeeFlag</td>
      <td>BOOLEAN</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>callbackUrl</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>memo</td>
      <td>STRING</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 출금 기록
```
GET /withdraw/history (서명)
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
      <td>coin</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
    <tr>
      <td>status</td>
      <td>INT</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>startTime</td>
      <td>LONG</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>endTime</td>
      <td>LONG</td>
      <td>아니요</td>
      <td></td>
    </tr>
    <tr>
      <td>limit</td>
      <td>INT</td>
      <td>아니요</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 출금 취소
```
POST /withdraw/cancel (서명)
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
      <td>withdrawId</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
  </tbody>
</table>

### 출금 정보
```
GET /withdraw/info (서명)
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
      <td>withdrawId</td>
      <td>STRING</td>
      <td>예</td>
      <td></td>
    </tr>
  </tbody>
</table>
