TRTC[コンソール](https://console.cloud.tencent.com/rav)に進み、確認・監視する必要のあるアプリケーション名をクリックして、アプリケーションの詳細ページに進みます。左側ナビゲーションバーで【監視ダッシュボード】を選択すれば、**通話品質**を確認できます。
通話品質の**クリックして詳細を表示**リンクをクリックすれば、この通話の詳細な指標の監視曲線を確認できます。詳細は以下のとおりです。

## ユーザーセクション
このセクションでは、アニメーションを使用してオーディオ・ビデオデータのフローを表します。左側のユーザーがオーディオ・ビデオデータをクラウドにアップロードすると、データはCVMで中継されて右側のユーザーに届きます。


## オーディオ・ビデオのビットレート
- **送信側ビットレート**
1秒あたりに送信されるオーディオ・ビデオデータ量のことで、単位はkbpsです。ビットレートが800kbpsの場合、1分間に生成されるデータ量は約800k * 60s / 8 = 6Mバイトとなります。
- **受信側ビットレート**
1秒あたりに受信されるオーディオ・ビデオデータ量のことで、単位はkbpsです。通常、値が小さいほど画像が不鮮明になります。



## ビデオフレームレート

- **送信側フレームレート**
送信側フレームレートとは、1秒あたりにエンコードされる画像のフレーム数のことです。推奨値は15FPSで、この値だと画像が大変滑らかになり、また、1秒あたりのフレーム数が多すぎることで、それぞれの画像の明瞭度が低下することはありません。
- **受信側フレームレート**
受信側フレームレートとは、1秒あたりに受信するビデオのフレーム数のことです。値が大きいほど画像が滑らかになり、値が小さいほど画像にラグが発生します。通常、フレームレートが5FPS未満になると、ラグが発生していることをユーザーもはっきりと感じるようになります。



## ネットワークパケット損失率(LOSS)
ネットワークパケット損失率(LOSS)は、値が低いほど良いです。例えば、パケット損失率が0％であれば、ネットワークの状態が最適であることを示し、パケット損失率が30％であれば、SDKとクラウドサーバー間で10パケットあたり3個のパケット破棄があることを意味します。



## ネットワーク遅延(RTT)
ネットワーク遅延(RTT)とは、SDKとサーバーのやり取りの所要時間を表し、値が小さいほど良いです。通常、RTT50ミリ秒未満が望ましい状態で、RTTが100ミリ秒を超えると大きな通話遅延が発生します。



## CPU使用率
「アプリCPU」はお客様のアプリのCPU使用量、システムCPUは現在のシステムの合計CPU使用量のことです。


## メモリ使用量

アプリのメモリ使用量のことです。最新のリアルタイムOS（iOS、Android、Windows）には複雑で複数のポリシーを持った仮想メモリ制御メカニズムがあるため、実際のメモリ使用量は変動します。


