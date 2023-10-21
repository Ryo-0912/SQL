参照 : https://deep-blog.jp/engineer/14781/

使い方 : カラムと文字列を結合したい場合や、複数のカラム同士を結合するのに使用

例

```sql
SELECT CONCAT(year, '-', month, '-', day) FROM user

#=> 2019-06-08
```

例

```php
->selectRaw("CONCAT(applicants.first_name, ' ', IFNULL(CONCAT(applicants.middle_name, ' '), ''), applicants.last_name) name")
```
