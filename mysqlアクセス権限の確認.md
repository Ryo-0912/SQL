参照 : https://qiita.com/akki-memo/items/357c1547a465af1c79d6 ,

https://www.wakuwakubank.com/posts/322-mysql-access-host/, 

https://style.potepan.com/articles/18387.html

表題のエラー内容は、アクセス権がないという内容のエラー。

アクセス権の確認

```php
**SELECT user, host, db FROM mysql.db;**

+---------------+-----------+--------------------+
| user          | host      | db                 |
+---------------+-----------+--------------------+
| docker        | %         | test               |
| mysql.session | localhost | performance_schema |
| mysql.sys     | localhost | sys                |
+---------------+-----------+--------------------+
```

この出力結果の意味

host ⇒ **`%`** : これは「どこからでもアクセス可能」の意味。

host ⇒ [`**localhost**`](http://localhost) : これは「ホスト名はlocalhostでないとアクセスできない」の意味。
