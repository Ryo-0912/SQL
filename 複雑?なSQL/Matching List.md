userテーブル u 

| id | name | sex |
| - | - | - | 
| 1 | 高麗 | １ |
| 2 | 藤田 | 1 |
| 3 | 福島 | 1 |
| 4 | 前澤 | 2 |
| 5 | 諸橋 | 1 |


user_matching um

| id | user_id | to_user_id |
| - | - | - | 
| 1 | 1 | 2 |
| 2 | 1 | 3 |
| 3 | 4 | 1 |
| 4 | 2 | 5 |
| 5 | 3 | 4 |

user_block ub

| id | user_id | to_user_id |
| - | - | - | - |
| 1 | 1 | 2 | 2 |
| 2 | 1 | 4 | 2 |
| 3 | 2 | 3 | 1 |
| 4 | 5 | 1 | 1 |


この3つのテーブルに対して、次のSQLを実行する。

```
SELECT
    um.*,
    ub.*
FROM
    user_matching AS um
JOIN

    // user_matching内で、user_id or to_user_id = 1のデータだけに絞り込む(今回はユーザ id = 1のマッチングリストを取得したいから) ①
    user ON (user.id = um.user_id AND um.user_id != 1) OR (user.id = um.to_user_id AND um.to_user_id != 1)
LEFT JOIN

    // ①の絞り込み済みテーブルに対して、ユーザ id = 1のユーザがブロック or ミュートしている(もしくは、されている)かの情報を、外部結合で①テーブルにくっつける
    user_block AS ub ON
    (ub.target_user_id = um.user_id AND ub.user_id = 1 AND ub.type IN (1, 2)) OR
    (ub.user_id = um.to_user_id AND ub.target_user_id = 1 AND ub.type = 1) OR
    (ub.user_id = um.user_id AND ub.target_user_id = 1 AND ub.type = 1) OR
    (ub.target_user_id = um.to_user_id AND ub.user_id = 1 AND ub.type IN (1, 2))
WHERE
    (um.user_id = 1 AND user.user_status IN (1, 2)) OR
    (um.to_user_id = 1 AND user.user_status IN (1, 2));
```

このSQLを実行すると、以下のような結合テーブルが取得できる。

| id | user_id | to_user_id |
| - | - | - | - | - | - | - |
| 1 | 1 | 2 | 1 | 1 | 2 | 2 |
| 2 | 1 | 3 | NULL | NULL | NULL | NULL |
| 3 | 4 | 1 | NULL | NULL | NULL | NULL |


この結果から、取得すべきデータはユーザ id = 1が、ブロックもミュートもしていないorされていないデータなので、下二つのデータを取得すればいい。
