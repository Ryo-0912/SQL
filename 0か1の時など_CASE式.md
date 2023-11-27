フォローされているか、ブロックされているかなど「されている or されていない」といったようにどちらかの選択肢の時は以下のようにCASE式を使うと良い

**CASE式はSELECTやUPDATEで使う。**

```
SELECT
  CASE
      WHEN [条件] THEN [条件を満たしたら表示する内容]
      ELSE [条件を満たしていなかったら表示する内容]
  END
FROM [テーブル名]
WHERE [条件];
```

例

```
SELECT ID,Name,Japanese,
  CASE WHEN Japanese >=  90  THEN '合格'
       WHEN Japanese <   90  THEN '不合格'
       ELSE null
   END AS Result
FROM PointList;

/* 結果（Result列の値がJapanese列の値によって決定される）
+----+------+----------+--------+
| ID | Name | Japanese | Result |
+----+------+----------+--------+
|  1 | 佐藤 |      100 | 合格   |
|  2 | 鈴木 |       90 | 合格   |
|  3 | 高橋 |       85 | 不合格 |
|  4 | 中村 |       90 | 合格   |
|  5 | 小林 |       70 | 不合格 |
|  6 | 山口 |       90 | 合格   |
|  7 | 田中 |       70 | 不合格 |
|  8 | 伊藤 |       70 | 不合格 |
+----+------+----------+--------+
*/
```

例　入れ子

```
SELECT
  name,
  CASE
      WHEN name="加藤" THEN
        CASE
            WHEN 80 <= point THEN "合格"
            ELSE "不合格"
        END
      ELSE "対象ではない"
  END
FROM user;

/* 結果
+--------+--------------------+
| name   | [条件式]            |
+--------+--------------------+
| 山田   | 対象ではない          |
| 鈴木   | 対象ではない          |
| 加藤   | 不合格               |
| 田中   | 対象ではない          |
+--------+--------------------+
*/
```

例 UPDATE

```
UPDATE user SET
  point =
    CASE
        WHEN 80 <= point THEN point*2
        ELSE 0
    END;

/* 結果
+------+--------+-------+
| id   | name   | point |
+------+--------+-------+
|    1 | 山田   |   160 |
|    2 | 鈴木   |   180 |
|    3 | 加藤   |     0 |
|    4 | 田中   |     0 |
+------+--------+-------+
*/
```
