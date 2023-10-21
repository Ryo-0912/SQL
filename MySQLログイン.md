参照 : https://style.potepan.com/articles/18389.html

オプションの使い方

### ●**`-p(パスワード)`**を使いたい時 ⇒ 空白なしで指定する

NG例

```php
mysql -u root -p my-secret-pw
```

空白開けると、`my-secret-pw`をパスワードではなく、DB名と認識してしまう。

OK例

```php
mysql -u root -pmy-secret-pw
```

空白開けると、`my-secret-pw`をパスワードと認識される。

●-uを使いたい時 ⇒ 空白あってもなくてもOK

### ●`-h`は`localhost`指定ではログインできるが、`127.0.0.1`ではログインできない。

MySQLでは、内部的にlocalhostと127.0.0.1は別のものとして扱われます。

そのため、localhost指定ではログイン成功、127.0.0.1指定だとaccess deniedでログイン失敗というケースが考えられます。

```
% mysql -u root -h localhost -pmy-secret-pw
mysql: [Warning] Using a password on the command line interface can be insecure.Welcome to the MySQL monitor.  Commands end with ; or \g.
:
:
mysq> exit
Bye

% mysql -u root -h 127.0.0.1 -pmy-secret-pw
mysql: [Warning] Using a password on the command line interface can be insecure.ERROR 1045 (28000): Access denied for user 'root'@'127.0.0.1' (using password: YES)
```

具体的には、**localhost指定でログインすると「root@localhost」**、**127.0.0.1指定だと「root@%」**という別ユーザとして扱われます。

例えば、セキュリティを考慮した運用面から、「root@%」ユーザを削除している環境だと、127.0.0.1指定時のみログインに失敗します。
