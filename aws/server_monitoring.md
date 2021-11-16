# AWS Hands-on for Beginners 監視編 サーバーのモニタリングの基本を学ぼう
## 元ネタ
https://pages.awscloud.com/JAPAN-event-OE-Hands-on-for-Beginners-monitoring-2020-reg-event-CP_707.html
## 01.監視アーキテクチャの概要
### なぜ監視が必要なのか？
システムのリソース不足、アプリケーションの稼働を確認することで、ユーザー体験を損なわないようにするため
### チェックすべき代表的な監視項目
1. リソース監視
2. ログ監視
3. APM
4. 外形監視
5. セキュリティ
6. コスト
## 02.基礎知識のおさらいと事前準備
- VPC
- EC2
- ELB
- RDS
- CloudFormation
  - テンプレを使用してAWSリソースをプロビジョニングできるサービス
  - プロビジョニングされたリソースに対する変更／削除も可能
  - 追加料金なし（リソースのみ発生）
### ハンズオン、CloudFormation準備
- 事前にサンプルファイルダウンロード
- スタックの作成
- テンプレートの準備完了
- テンプレートファイルのアップロード(monitoring-1.yamlを選択)
- 次へ
- スタックの名前を入力(monitoring-1)
- 次へ
- 次へ
- チェックボックスON、スタックの作成(作成に約10分)
※東京コケたので、バージニア北部で実行した
## 03.Amazon CloudWatchの概要
モニタリングに関する様々な機能を提供
### ハンズオン、CloudFormationにwordpressインストール
- 各サーバーのDNSにアクセス
- wordpress設定
## 04.Amazon CloudWatchのハンズオン ①EC2、ALB、RDSのメトリクス確認
### メトリクス監視がなぜ必要か
#### よくある問題
- サーバーが過負荷で応答できない
- サービスが使えない
- 良い顧客体験を提供できない
#### 測定すべき項目
- リソース監視（例：CPU使用率、メモリ使用率）
- アプリケーション性能管理（例：アプリケーションの処理時間）
### CloudWatch Metrics
CloudqWazrchに発行されたメトリクスを収集し、統計を取得
### 用語
#### メトリクス
- CloudWatchに発行された時系列のデータポイントのセット
- メトリクス名を持つ
- データポイントはタイムスタンプと測定単位を保持
- メトリクスは作成されたリージョニのみ存在
#### 名前空間
- CloudWatchメトリクスのコンテナ
- 異なる名前空間のメトリクスは相互に切り離される
- AWSサービスではAWS/`<service>`が使用される（例：AWS/EC2）
#### ディメンション
- メトリクスを一意に識別する名前/値のペア（例：InstanceId=i-1234545678）
### ハンズオン CloudWatchメトリクス
#### EC2
- インスタンスメトリクス
- CPUUtilization、NetworkIn、NetwrorkOutなど確認
- グラフ上部、カスタムから現地タイムゾーンに変更
#### ApplicationELB
- AppELB別、TG別メトリクス
- HealthHostCount、HTTPCode_Taget_4XX_Count、HTTPCode_Taget_2XX_Count
#### RDS
- データベース別メトリクス
- WriteIOPS、ReadIOPS
- グラフ化したメトリクスから期間を1分に設定
#### CWAgent（カスタムメトリクス）
- ImageId,InstanceId,InstanceType,device,fsty.pe,...
- disk_used_parcent（ディスク使用率）
- ImageId,InstanceId,InstanceType
- mem_used_parcent（メモリ使用率）
## 05.Amazon CloudWatchのハンズオン ②EC2のディスク使用率90%以上時のアラート発報
### アラートが何故必要か
システムやシステムのコンポーネントの振る舞いに変化が起きた際にビジネスへの影響を無くすため
### CludWatch Alarms
- CludWatch Metricsをモニタリングしてアラームを発行可能
- 条件を指定して、自動アクションを実行が可能
  - 例えば、CPU利用率が80%を超えた時にアラートメールを通知等
- アラームの状態はOK,ALAEM,INSUFFICIENT_DATAの３種類
### ハンズオン CloudWatchアラーム
- アラーム
- メトリクスの選択
- CWAgent
- ImageId,InstanceId,InstanceType,device,fsty.pe,...
- disked_used_percent（pathが/）
- 条件設定
- 種類静的、しきい値90
- 次へ
- アラーム状態
- 新しいトピック作成
- トピック名入力、Eメール設定
- Auto Scalingアクション、EC2アクションで挙動が設定できる（今回はしない）
- 次へ
- アラーム名設定
- 次へ
- アラームの作成