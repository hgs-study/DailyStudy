### KAS(Klaytn API Service)
------------------
```
ㅇ KAS 링크
ㅇ KAS API Basic
ㅇ TokenHistory 
ㅇ Wallet
```
ㅇ KAS 링크
------
  + KAS Console
    + https://console.klaytnapi.com/ko/dashboard
  + KAS 용어
    + https://ko.docs.klaytn.com/misc/glossary
  + KAS Wallet - TestNet(Baobab)
    +  https://baobab.wallet.klaytn.com/
  + API 호출
    + https://docs.klaytnapi.com/getting-started/api 

ㅇ KAS API Basic
-------
  + KAS는 Basic HTTP Auth를 사용합니다. 모든 요청은 반드시 올바른 Authorization 헤더를 가져야 하며 KAS 사용자는 access key ID를 username으로, secret access key를 password로 사용하여 Basic Auth에 사용할 자격증명을 생성합니다.
  + Authorization
  + x-chain-id (cypress : 8217 or baobob : 1001)

ㅇ TokenHistory 
------
  + Token History API는 KLAY, FT (KIP-7, Labeled ERC-20), NFT (KIP-17, Labeled ERC-721), MT (KIP-37, Labeled ERC-1155) 토큰 정보, 이들 토큰을 주고받은 기록을 조회하는 기능을 제공합니다. 특정 EOA가 KLAY를 주고받은 기록을 확인하거나 EOA가 가지고 있는 NFT 정보를 불러오는 등 Token History API를 다양하게 활용할 수 있습니다.
  + EOA API : https://th-api.klaytnapi.com/v2/transfer/account/{address}

ㅇ Wallet
------
  +
  + Wallet API : https://refs.klaytnapi.com/ko/wallet/latest#section/Introduction
