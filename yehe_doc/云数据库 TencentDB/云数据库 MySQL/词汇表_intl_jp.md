<div class="tab_list">
<ul>
    <li><a href="#A-E">A-E</a></li>
    <li><a href="#F-J">F-J</a></li>
    <li><a href="#K-O">K-O</a></li>
    <li><a href="#P-T">P-T</a></li>
    <li><a href="#U-Z">U-Z</a></li>
</ul>
</div>

<span id="A-E"></span>
## A-E 
**バックアップストレージ**
データベースのデータやログなどのバックアップの基盤となるストレージリソースを永続的に保存するために使用されます。
**読み取り/書き込み分離**
マスターインスタンスは追加、更新、削除などのトランザクション操作（INSERT、UPDATE、DELETE）を実行でき、スレーブインスタンス（読み取り専用）はSELECTクエリ操作を実行できます。

<span id="F-J"></span>
## F-J 
**高い信頼性**
 高い信頼性は、サービスの高可用性を維持するためにダウン・タイムの時間と頻度を削減するように特別に設計されたシステムを表すために使用されます。
**リレーショナルデータベース**
リレーショナルデータの構造に基づいて連携及び構成されたデータベースです。リレーショナルデータベースのモデルは、複雑なデータ構造を単純な二項関係（すなわち、二次元の表形式）にまとめるものです。リレーショナルデータベースでは、データに対する操作のほとんどが1つ又は複数のリレーショナルテーブルで行われ、これらの関連テーブルの分類、結合、接続又は選択などの操作によってデータベースが管理されます。一般的なリレーショナルデータベースには、Oracleデータベース、MySQLデータベース、MariaDBデータベース、SQL Serverデータベース、Accessデータベース、DB2、PostgreSQL、Informix、Sybaseなどがあります。
**ソリッドステートドライブ**
略してSSDといい、ソリッドステート電子記憶チップアレイで作られたハードディスクで、制御ユニットと記憶ユニット（FLASHチップ、DRAMチップ）から構成されています。SSDはインターフェースの規格と定義、機能、及び使用方法が一般的なハードディスクドライブとまったく同じであり、製品のフォームファクタとサイズも一般的なディスクドライブと完全に一致しています。軍事、車載、工業制御、映像監視、ネットワーク監視、ネットワーク端末、電力、医療、航空、ナビゲーション設備などの分野に広く応用されています。

<span id="K-O"></span>
## K-O 
**論理バックアップ**
SQL言語を使用してデータベースからデータを抽出し、バイナリーファイルに保存するプロセスです。論理バックアップとは、ソフトウェア技術を使用してデータベースからデータをエクスポートし、出力ファイルに書き込むことです。通常、このファイルの形式は元のデータベースのファイル形式とは異なり、単に元のデータベースのデータ内容のイメージです。したがって、論理バックアップファイルは、データベースの論理的な復元（データインポート）にのみ使用され、データベースの元のストレージ特性に基づいて物理的に復元することはできません。論理バックアップは、通常、増分バックアップに使用されます。つまり、前回のバックアップ以降に変更されたデータをバックアップします。
**コールドバックアップ**
システムがシャットダウンまたはメンテナンスされているときに使用されるバックアップ方法です。この場合、バックアップされたデータは、システム内のこの時点のデータと完全に一致します。

<span id="P-T"></span>
## P-T 
**QPS**
QPS（1秒あたりのクエリ数）は、特定の問合せサーバが指定された時間にどれだけのトラフィックを処理したかを測定するものです。 
**ホットバックアップ**
システムが正常に動作しているときに使用されるバックアップ方法です。この場合、システム上のデータはリアルタイムで更新されるため、バックアップされたデータはシステム上の実際のデータと比較してある程度遅れている可能性があります。
**データレプリケーション**
MasterからSlaveへのデータのレプリケーションは、強い同期レプリケーション、半同期レプリケーション及び非同期レプリケーションのいずれか方法で行われます。
**データベースストレージ**
データベースのデータとログの基盤となるストレージリソースを永続的に保存するために使用されます。
**データベース管理者**
データベース管理者（DBA）は、データベースを管理する責任者です。DBAは、専用のソフトウェアを使用してデータを保存及び整理します。この役割には、容量計画、インストール、構成、データベース設計、コンフィグレーション、パフォーマンス監視、セキュリティ、トラブルシューティング及びバックアップとデータ復元が含まれます。
**データベース接続数**
データベースインスタンスに接続しているクライアントセッションの数を指します。
**データベースのマイグレーション**
サービスの変化に伴って、データベースは、アプリケーションサービスに伴って、ローカルIDCからクラウドへ、又はクラウドから別のクラウドへなど、ある環境から別の環境へと移行する必要があります。
**データベースインスタンス**
データベースインスタンスは、クラウド内で単独で動作しているデータベース環境です。これはTencentDBの基本的な構築データブロックです。データベースインスタンスにはデータベースのユーザーによって作成された複数のデータベースを含めることができ、スタンドアロン・データベースのインスタンスと同じのクライアントツール及びアプリケーションを使用してアクセスできます。
**データベースエンジン**
データベースエンジンは、データの保存、処理及び保護のためのコアサービスです。データベースエンジンを使用すると、アクセス権限を制御してトランザクションを迅速に処理できるため、大量のデータを処理する必要がある企業内のほとんどのアプリケーションの要件に対応できます。データベースエンジンは各データベースインスタンスでサポートされています。

<span id="U-Z"></span>
## U-Z
**物理バックアップ**
実際にデータベースを構成するOSファイルを、通常はディスクからテープにコピーするバックアッププロセスです。物理バックアップは、コールドバックアップとホットバックアップに分けられます。
