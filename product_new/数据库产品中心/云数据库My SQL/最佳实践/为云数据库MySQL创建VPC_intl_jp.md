Tencent Cloudでは、クラウドデータベースをホスティングするフラットフォーム、すなわちTencent Cloud VPCを用意しております。VPCでTencent Cloudのリソース（例えば、Tencent Cloudクラウドデータベースインスタンス）を起動することができます。VPC詳細については [Virtual Private Cloud](https://intl.cloud.tencent.com/document/product/215/535)をご参照ください。  

同じVPCで実行されているクラウドデータベースインスタンスをWebサーバーとデータ共有するソリューションがよく用いられています。本マニュアルでは、このソリューションに対してVPCを作成し、また、クラウドデータベースをVPCに追加して併用します。

## 1. VPCの作成・サブネットとルートテーブルの初期化
[VPC](https://cloud.tencent.com/document/product/215/20109)は少なくとも１つのサブネットを含みます、サブネットのみでクラウドサービスリソースを追加することができます。
1.  [VPCコンソール](https://console.cloud.tencent.com/vpc/vpc?rid=1)にログインし、左側の欄から【Virtual Private Cloud】ページを選択します。
2. リストの上方に地域を選択し、【新規作成】をクリックするとVPCを作成することができます。例えば、地域：華南地域（広州）を選択します。
3. VPC、サブネットの名称、およびCIDRを記入し、サブネットのアベイラビリティゾーンを選択し、【作成】をクリックすればいいです。


## 2. サブネットの作成
1つあるいは複数のサブネットを作成することができます。
1.  [VPCコンソール]にログインし、左側の欄から【サブネット】ページを選択します。
2.  リストの上方に地域とVPCを選択し、【新規作成】をクリックします。
3.  サブネットの名称、CIDR、アベイラビリティゾーン、関連するルートテーブルを記入し、【一行を追加する】をクリックすると、複数のサブネットを同時に作成することができます。エラーがないことを確認したら、【作成】をクリックすればいいです。

## 3. ルートテーブルに関連付けサブネットの新規作成
カスタムルートテーブルを作成し、ルーティングポリシーを編集し、次に指定するサブネットに関連付けます。サブネットに関連付けられているルートテーブルは、該当サブネットのアウトバウンドルートの指定に用いられます。
1.  [VPCコンソール]にログインし、左側の欄から【ルートテーブル】ページを選択します。
2. リストの上方に地域とVPCを選択し、【新規作成】をクリックします。
3.  ポップアップされたダイアログボックスに名称、属するネットワーク及び新規ルーティングポリシーを入力し、【作成】をクリックします。
3. ルートテーブルリストに戻ると、新規作成したルートテーブルが見られます。
4. 左側の欄から【サブネット】ページを選択し、このルートテーブルに関連付けるサブネットを選択し、【操作】列から【ルートテーブルの変更】をクリックして、関連付けます。

## 4. クラウドデータベースの追加
新規に購入したクラウドデータベースは、VPCで利用可能です。**ネットワークを選定すると、変更はできなくなります**ので、注意してください。
1.  [MySQL コンソール](https://console.cloud.tencent.com/cdb)にログインし、【新規作成】をクリックすると、購入ページが入ります。
2.  購入ページで【ネットワーク】オプションに、【Virtual Private Cloud】を選択し、すでに作成したVPCと対応するサブネットを選択すると、新規に購入したクラウドデータベースをVPCに追加します。

## 5. CVMの追加
新規に購入したCVMは、VPCで利用可能です。**ネットワークを選定すると、変更はできなくなります**ので、注意してください。
1. [CVMコンソール](https://console.cloud.tencent.com/cvm/index)にログインし、【新規作成】をクリックすると、購入ページに入ります。
2.  購入ページで【ネットワーク】オプションに、前のデータベースと同一のVPCを選択すると、新規に購入したCVMをクラウドデータベースと同一のVPCに追加します。

