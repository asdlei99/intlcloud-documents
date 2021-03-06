## 機能の説明
Content Delivery Network(CDN) はユーザーのためにIP帰属照会ツールを提供しています。ユーザーはこのツールで指定されたIPがTencent loud CDNのグローバル加速ノードであるかどうか、IPの所在する加速サービスリージョン、省及びキャリア情報を確認することができます。
## ユースケース
障害処理類のケースではこのツールはトラブルシューティングに使われることができます。ユーザーアクセスに異常が発生した場合は、クライアントリクエストが実際にアクセスしたIPはここで照会します。
- Tencent Cloud CDNに帰属していない場合は、ドメイン名の解析構成は異常である可能性があります。ドメイン名の解析のプロバイダーでCNAMEの構成が正常であるかどうかを確認します。
- Tencent Cloud CDNに帰属している場合は、ノードのサービス状態を照会することにより、ノードのオンライン/オフラインによるリクエスト中断があるかどうかを確認します。

## 操作ガイド
### 照会の方法
 [CDN コンソール](https://console.cloud.tencent.com/cdn)にログインし、左側ディレクトリの【診断ツール】>【IP 帰属照会】を選択すると、機能ページが表示されます。
![](https://main.qcloudimg.com/raw/7c72a39a1c0f33e633057d02af9c3a6f.png)
### ご利用の規約
- テキストボックスに検証する複数のIPアドレスを1行に1つずつ入力します。
- 最大20個のIPまでを一括検証できます。
- IPv4とIPv6アドレスの検証をサポートしています。
- グローバルにおける加速ノードの検証をサポートしています。中国国内のノードの場合は所在する省のキャリアのデータを戻し、海外のノードの場合は所在する国のデータを戻します。
- ノードの**過去3時間**のサービス状態の変動状況を確認できます。オンライン/オフラインの変動がある場合は、対応する操作時間を照会できます。

## 利用例
### IPが中国国内に帰属する
![](https://main.qcloudimg.com/raw/92a04bfdc0905c9be0465d3dc4825dd3.png)
### IPが海外に帰属する
![](https://main.qcloudimg.com/raw/6a2e1b6f94362d5508ed98a52bd2d125.png)
### IPv6帰属照会
>
![](https://main.qcloudimg.com/raw/7e88553e81f01e86fd4325c3d433fbca.png)







