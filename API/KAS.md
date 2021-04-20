### Klaytn-API
-------
```
Rest API를 활용한 Klaytn Node 접근
```
+ 기본 헤더
  + Authorization (인증)
  + x-chain-id (Klaytn Network Chain Id)
+ 지갑 생성
+ 토큰 히스토리 조회


### KAS(Klaytn API Service)
------------------
```
ㅇ KAS 링크
ㅇ KAS API Basic
ㅇ TokenHistory 
ㅇ Wallet
```

### ㅇ KAS 링크
------
  + KAS Console
    + https://console.klaytnapi.com/ko/dashboard
  + KAS 용어
    + https://ko.docs.klaytn.com/misc/glossary
  + KAS Wallet - TestNet(Baobab)
    +  https://baobab.wallet.klaytn.com/
  + API 호출
    + https://docs.klaytnapi.com/getting-started/api 

### ㅇ KAS의 단위
-------
  + Klaytn은 KLAY에 다음과 같은 단위 시스템을 사용합니다.
    + peb는 가장 작은 화폐 단위입니다.
    + ston은 Gpeb의 별명으로 편의를 위해 만들어졌습니다.
    + KLAY는 10^18 peb입니다.

### ㅇ KAS API Basic
-------
  + KAS는 Basic HTTP Auth를 사용합니다. 모든 요청은 반드시 올바른 Authorization 헤더를 가져야 하며 KAS 사용자는 access key ID를 username으로, secret access key를 password로 사용하여 Basic Auth에 사용할 자격증명을 생성합니다.
  + Authorization
  + x-chain-id (cypress : 8217 or baobob : 1001)
  


### ㅇ JSON-RPC API
-------
  + 이 페이지는 Node API로 Klaytn Endpoint Node가 제공하는 JSON-RPC 함수들을 호출하는 법을 안내합니다.
  + JSON-RPC API 조회 : https://ko.docs.klaytn.com/bapp/json-rpc/api-references/klay
  + 기본 Node API : https://node-api.klaytnapi.com/v1/klaytn
  + 요청 Body에 원하는 method 붙여서 요청 (ex : klay_getBalance)

### ㅇ TokenHistory API
------
  + Token History API는 KLAY, FT (KIP-7, Labeled ERC-20), NFT (KIP-17, Labeled ERC-721), MT (KIP-37, Labeled ERC-1155) 토큰 정보, 이들 토큰을 주고받은 기록을 조회하는 기능을 제공합니다. 특정 EOA가 KLAY를 주고받은 기록을 확인하거나 EOA가 가지고 있는 NFT 정보를 불러오는 등 Token History API를 다양하게 활용할 수 있습니다.
  + EOA API : https://th-api.klaytnapi.com/v2/transfer/account/{address}


### ㅇ Wallet API
------
  #### Wallet
  + Wallet API : https://refs.klaytnapi.com/ko/wallet/latest#section/Introduction
  + 기본 API : https://wallet-api.klaytnapi.com/*
  + 계정 조회 : https://wallet-api.klaytnapi.com/v2/account
   
  #### Transaction
  + Transaction API : https://docs.klaytnapi.com/tutorial/wallet-transaction-api/wallet-transaction-klay /        https://refs.klaytnapi.com/ko/wallet/latest#operation/ValueTransferTransaction
  + 전송 API : https://wallet-api.klaytnapi.com/v2/tx/value (※ body에 submit 추가로 넣어줘야함)


### ㅇ KAS 용어 & 개념 참고
-------
  + https://velog.io/@hyunjeongshub/Klaytn-API-Service-KAS
  + https://seung-hwan.tistory.com/m/63?category=971426
