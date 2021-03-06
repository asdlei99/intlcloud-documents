スケーリングアクティビティーのキャンセルとは、時限タスクの時刻またはアラームスケーリングの条件に達したときにスケーリングアクティビティーがトリガーされますが、競合があり、スケーリングアクティビティーが強制的にキャンセルされることを意味します。

**競合原因：**
- 実行中のスケーリングアクティビティーがあります。
- スケーリンググループが冷却時間中です。

**スケーリングアクティビティーがキャンセルされた後、再試行されますか？**
- **アラームスケーリング**アクティビティーがキャンセルされた場合、再試行されません。ただし、アラームスケーリングの条件が引き続き確立されると、次のアラームスケーリングアクティビティーがトリガーされます。
- **時限タスク**は、目標インスタンス数、最大のスケーリング数、および最小のスケーリング数を定義するため、スケーリンググループはずっと再試行し、実際のインスタンス数が目標インスタンス数と一致させます。

>注：スケーリンググループは中断され、スケーリンググループはスケーリングアクティビティーを直接しようとしません。そのため、「スケーリングアクティビティー」レコードでは、キャンセルされたスケーリングアクティビティーは残りません。

