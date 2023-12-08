usersテーブル

| id | name | introduction | created_at | updated_at |
| --- | --- | --- | --- | --- |
| 1 | 松本人志 | お笑い芸人 | 2023-12-05 | 2023-12-05 |
| 2 | 新垣結衣 | 女優 | 2023-12-07 | 2023-12-07 |
| 3 | 広瀬すず | 女優 | 2023-12-10 | 2023-12-10 |

上記のようなテーブルがあるとき、このSQLを実行すると、

```jsx
SELECT 0 AS 'user_id', name, introduction FROM users WHERE id = 1 OR id = 2;
```

以下のような結果が得られる。

| user_id | name | introduction |
| --- | --- | --- |
| 0 | 松本人志 | お笑い芸人 |
| 0 | 新垣結衣 | 女優 |
