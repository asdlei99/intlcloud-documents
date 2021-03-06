### ネットワークタイプが異なります
CVMインスタンスとMySQLインスタンスのネットワークタイプが一致しない場合、CVMインスタンスはプライベートネットワーク経由でMySQLインスタンスに直接アクセスできません。
#### CVMインスタンスは専用ネットワーク（VPC）を使用し、MySQLインスタンスは基幹ネットワークを使用します
- 解決方法1（**推奨**）：MySQLインスタンスを基幹ネットワークからVPCネットワークに切り替えます。
>!
>- 切り替え後、プライベートネットワークで相互接続するには、両方が同じVPCネットワークでなければなりません。
>- 基幹ネットワークをVPCネットワークに切り替えた後は、元に戻すことはできません。
>- 切り替え後、VPCネットワークへのアクセスが直ちに有効になります。元の基幹ネットワークのアクセスは24時間保持されます。このインスタンスの関連するインスタンスを24時間以内にVPCネットワークに移行して、関連インスタンスのアクセスを確保してください。
- 解決方法2：基幹ネットワークのCVMインスタンスを再購入します（CVMインスタンスはVPCから基幹ネットワークへの移行をサポートしていません）。ただし、VPCネットワークは基幹ネットワークよりも安全性が高いため、VPCネットワークを使用することをお勧めします。
- 解決方法3：CVMインスタンスは、MySQLインスタンスのパブリックネットワーク接続用アドレスを使用してMySQLインスタンスに接続します。この方法では、パフォーマンス、セキュリティ、安定性が劣るため、VPCネットワークを使用することをお勧めします。

#### CVMインスタンスは基幹ネットワークを使用し、MySQLインスタンスは専用ネットワーク（VPC）を使用します
- 解決方法1（**推奨**）：CVMインスタンスを基幹ネットワークからVPCネットワークに移行します。詳細については、[CVMの移行例](https://intl.cloud.tencent.com/document/product/213/20278)をご参照ください。
 >!  
>- 切り替え後、プライベートネットワークで相互接続するには、両方が同じVPCネットワークでなければなりません。
>- 移行する前に、プライベートネットワークとパブリックネットワークのCLBおよびENIからバインド解除し、プライマリENIのセカンダリIPをリリースし、移行後にそれらを再バインドしてください。
>- 移行中、インスタンスを再起動する必要があります。他の操作を行わないでください。
>- 移行後、インスタンスの実行状態、プライベートネットワークへのアクセス、リモートログインが正常であるかを確認してください。
>- 基幹ネットワークがVPCネットワークに切り替えられると不可逆であり、CVMがVPCネットワークに切り替えられた後、他の基幹ネットワークのクラウドサービスと相互に接続されません。
- 解決方法2：基幹ネットワークを使用して相互接続します。
- 解決方法3：CVMインスタンスは、MySQLインスタンスのパブリックネットワーク接続用アドレスを使用してMySQLインスタンスに接続します。この方法では、パフォーマンス、セキュリティ、安定性が劣るため、VPCネットワークを使用することをお勧めします。


### プライベートネットワークは異なります
 デフォルトでは、CVMインスタンスとMySQLインスタンスのネットワークタイプがVPCネットワークであり、且つ両方が同じVPCネットワークにある場合、プライベートネットワークを通じて直接相互接続できます。異なるVPCにある場合は、次の方法でCVMとMySQLを相互接続できます。
- 解決方法1（**推奨**）：MySQLインスタンスをCVMインスタンスが存在するVPCネットワークに移行します。
具体的な操作：MySQLの[ネットワーク切り替え](https://intl.cloud.tencent.com/document/product/236/31915)を参照して、MySQLインスタンスのVPCネットワークをCVMインスタンスが存在するVPCネットワークに切り替えます。
- 解決方法2：2つのVPCネットワーク間にピアツーピア接続を確立します。
この方法を使用しないと、異なるVPCネットワークにあるCVMとMySQLは、パブリックネットワークを通じてのみ相互接続できます。この方式は、パフォーマンス、セキュリティ、安定性が劣ります。

### セキュリティグループが正しく設定されていません
CVMインスタンスとMySQLインスタンスのセキュリティグループが正しく設定されていない場合、CVMインスタンスはプライベートネットワークまたはパブリックネットワーク経由でMySQLインスタンスに直接アクセスできません。

#### CVMインスタンスセキュリティグループが正しく設定されていません
CVMインスタンスを使用してプライベートネットワーク経由でMySQLインスタンスにアクセスするには、CVMセキュリティグループにアウトバウンドルールを設定する必要があります。**アウトバウンドルールのターゲット設定が0.0.0.0/0でなく、プロトコルポートがALLでない場合**、MySQLインスタンスのプライベートネットワークIPとポートをアウトバウンドルールに追加する必要があります。
1. [セキュリティグループコンソール](https://console.cloud.tencent.com/vpc/securitygroup)にログインし、セキュリティグループをクリックして、CVMにバインドされるセキュリティグループの詳細ページに入ります。
2. 【アウトバウンドルール】ページを選択し、【ルールの追加】をクリックします。
IPアドレス（セグメント）およびオープンするポート情報（MySQLのプライベートネットワークアドレス）を入力して、オープン許可を選択します。

#### MySQLインスタンスセキュリティグループが正しく設定されていません
指定されたCVMインスタンスがプライベートネットワーク経由でMySQLインスタンスにアクセスするか、パブリックネットワーク経由でMySQLインスタンスにアクセスするには、MySQLセキュリティグループ内でインバウンドルールを設定する必要があります。**インバウンドルールのソース設定が0.0.0.0/0でなく、プロトコルポートがALLでない場合、**CVMインスタンスのプライベートネットワークIPまたはパブリックネットワーククライアントIPとポートをインバウンドルールに追加する必要があります。 
1. [セキュリティグループコンソール](https://console.cloud.tencent.com/vpc/securitygroup)にログインし、セキュリティグループをクリックして、MySQLインスタンスにバインドされるセキュリティグループの詳細ページに入ります。
2. 【インバウンドルール】ページを選択し、【ルールの追加】をクリックします。
アクセスを許可するIPアドレス（セグメント）とオープンするポート情報（MySQLプライベートネットワーク）を入力し、オープン許可を選択します。
>!  
>- MySQLのプライベートネットワークのデフォルトポートは3306で、カスタムポートもサポートされています。デフォルトのポート番号が変更された場合は、セキュリティグループ内でMySQLの新しいポート情報をオープンする必要があります。
>- パブリックネットワークを使用してMySQLインスタンスにアクセスする場合、セキュリティグループのインバウンドルールはMySQLインスタンスの3306ポートをオープンする必要があります。
>- パブリックネットワークを使用してMySQLインスタンスにアクセスする場合は、パブリックネットワーククライアントのIPアドレスをセキュリティグループのインバウンド規則に追加する必要があります。

