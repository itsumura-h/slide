---
marp: true
theme: default
---

# Ethereum インテグレーションの仕組みとロードマップ

---

# ICP for Ethereum


---

![](../eth_integeration/image/ethcc.jpg)

---

![](../eth_integeration/image/ethcc2.jpg)

---


![](../eth_integeration/image/ethcc3.jpg)

---


![](../eth_integeration/image/ethcc4.jpg)

---

## Dfinityが考える現在のEthereumの問題

- Ethereum上のDappのフロントエンドが中央集権組織であるAWSやCloudflareにホスティングされている
   - それにより対検閲耐性がない
- メタバースやGameFiと言われるものが中央集権組織であるAWSやGCPにホスティングされている
  - 開発者がバックドアを仕込んだり、データベースにアクセスして恣意的にデータを変更できる
- Dapp開発のガバナンスが分散化していない
  - ユーザーの意思とは異なるアップデートが追加される恐れがある
- オンチェーンでのデータ取得が中央集権的なオラクルに依存している
- オンチェーン情報の取得が中央集権的なクラウド、またはInfuraのようなサービス提供者に依存している


---

## Ethereum上のDappのフロントエンドが中央集権組織であるAWSやCloudflareにホスティングされている
## →ICP上に静的サイトをホスティングする

---

### Uniswap on ICP
![](./image/uniswap-on-icp.jpg)

---

カスタムドメインを設定することもできる

![](./image/custom-domain.jpg)

---

## メタバースやGameFiと言われるものが中央集権組織であるAWSやGCPにホスティングされている
## →メタバース on ICP

---

![](./image/boomdao.jpg)

---

## Dapp開発のガバナンスが真に民主的になっていない
## →サービスナーバスシステムによるガバナンスの分散化

---

## オンチェーンでのデータ取得が中央集権的なオラクルに依存している
## オンチェーン情報の取得が中央集権的なクラウド、またはInfuraのようなサービス提供者に依存している

## →分散型オラクル on ICP

---

## Orally
![](./image/orally.jpg)

---

## Chainsight
![](./image/chainsight.jpg)

---

![](./image/sakai-chainsight.jpg)

---

![](./image/shumpei-chainsight.jpg)

---

# Ethereum統合

---

## Ethereum インテグレーションとは何か？

- 1. プロトコルレベルの統合
   - 1. フェーズ1
      - HTTPアウトコールの技術を使って、ICP上のキャニスターからEthereumの既存のRPCサーバーに対して通信する
   - 2. フェーズ2
      - ICP上にEthereumのフルノードとRPCサーバーを立て、これらのサブネットとキャニスターが通信する
- 2. ckETH & ckERC20
   - 分散化され、暗号学的に安全なブリッジをICP上に作り、ICPにEthereumの資産を持ち込む
- 3. EVM on ICP
   - ICP上でEVMが動くようにする

---
プロトコルレベルの統合-フェーズ1については、2023年中での完了を目指して頑張っているよう

![](./image/eikichi.jpg)

---

# ckETH & ckERC20

---

## ICPの仕組み

- 1つのサーバー → ノード
- ノードをネットワークで繋げたもの → サブネット
- サブネットは世界中にたくさんある
- キャニスターはサブネットにデプロイされる

---

![](https://res.cloudinary.com/zenn/image/fetch/s--jivBuvTQ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://miro.medium.com/v2/resize:fit:1400/1%2AmN3znV92PdK7T_OA4ETnjg.jpeg)

---

## Chain Keyの仕組み

- ノードは秘密鍵の破片を持つ
- サブネットは一つの公開鍵を持つ
- キャニスターはサブネット内の複数のノードで実行される
- 複数のノードが同じ実行結果になると、それぞれのノードで秘密鍵の破片で署名をする（しきい値署名）
- サブネットの公開鍵で署名を検証できる

---

![width:1200](./image/icp-subnet.png)

---

## Chain key TXの仕組み

- ICPのサブネットがICPの秘密鍵・公開鍵と、ETHの秘密鍵・公開鍵の両方を持つ
- ICPのキャニスターはEAOのウォレットとして、署名付きトランザクションを作ることができる

---

![height:700](./image/cketh.png)

---

## ETHのスマートコントラクトのブリッジ
- スマートコントラクト & リレイヤー
  - ハッキングが起きがち
- サーバー上のアプリケーション & ウォレット
  - 誰かが秘密鍵を知れる?マルチシグされていればいいが…


## ICP上のブリッジキャニスター
- 秘密鍵が絶対にノードに分散されているので安全
- ただのEOAのウォレットなのでスマートコントラクト起因のハッキングは起きない

## →ETHとICP間の交換に安全なブリッジが作れる

---

## IC Light House
- 板取引を行えるICDex
- エクスプローラーであるICHouse
- ThirdWebのようにノーコードでトークン発行ができるicTokens

---

![](./image/icDex.jpg)

---

![](./image/icExplorer.jpg)

---

![width:1200](./image/icTokens.png)

---
## icETHの進捗

![](./image/icRouter.jpg)

---

![width:1200](./image/iclighthouse-bridge.jpg)

---

![height:700](./image/iclighthouse-bridge2.jpg)

---

![height:700](./image/iclighthouse-bridge3.jpg)

---

![height:700](./image/iclighthouse-bridge4.jpg)

---

![height:700](./image/iclighthouse-bridge5.jpg)

---


## EVM on ICP by Bitfinity

- ブロックタイムが1秒
- ファイナリティが2秒
- トランザクションコストは0.02ドル

---

```
RPC URL
https://testnet.bitfinity.network/

Chain ID
355113

Currency Symbol
BFT

Block Explorer URL
https://explorer.bitfinity.network/
```

---

## Chapswap on Bitfinity

![](./image/chapswap1.jpeg)

---

![](./image/chapswap2.jpeg)

---

全てをSolidityでやろうとするのではなく…

||データベース操作|アプリケーション開発|
|---|---|---|
|既存金融|COBOL|Java|
|Web2|SQL(RDB)|PHP/Ruby/Python/TS...|
|Web3|Solidity(ETH)|WASM(ICP)|

---

## お前誰よ？

![](./image/whoareyou.jpg)
