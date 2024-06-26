実行SQLの前に```EXPLAIN```をつけると、そのSQLがどのようなスキャンをしているかがわかる。

# typeの種類
| type |	検索方法 |
----|---- 
| ALL |	フルスキャン、全件検索 |
| index |	フルインデックススキャン |
| const |	主キーやユニークキーを使って検索 |
| eq_ref |	↑と類似。表結合しているものに使われると表示される |
| ref |	ユニークじゃないインデックスを使って検索 |
| range |	インデックスを使って検索 |

例

- フルスキャン

```
mysql> EXPLAIN SELECT * FROM party where id = 1;
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | party | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)

```

- その他

```
mysql> EXPLAIN SELECT * FROM party where id = 1;
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | party | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```
