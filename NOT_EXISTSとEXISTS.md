```php
SELECT u.birth_date, u.prefecture, ulh.user_id, ulh.to_user_id
FROM bridge_staging.user u
INNER JOIN bridge_staging.user_like_history ulh
ON u.id = ulh.to_user_id
WHERE ulh.user_id = 8
AND NOT EXISTS (
    SELECT *
    FROM bridge_staging.user_block ub
	-- ブロックされた人 = いいね送った人 &　ブロックした人 = いいね送られた人(相手に自分がブロックされている)
    WHERE (ub.target_user_id = ulh.user_id AND ub.user_id = ulh.to_user_id)
    -- ブロックした人 = いいね送った人 & ブロックされた人 = いいね送られた人(自分が相手をブロックしている)
    OR (ub.user_id = ulh.user_id AND ub.target_user_id = ulh.to_user_id))
AND NOT EXISTS (
    SELECT *
    FROM bridge_staging.user_matching um
	-- マッチングした人 = いいね送られた人　＆　マッチングされた人 = いいね送った人(id = 8の人がマッチングした人)
    WHERE (um.user_id = ulh.to_user_id AND um.to_user_id = ulh.user_id)
    -- マッチングした人 = いいね送った人　＆　マッチングされた人 = いいね送られた人(id = 8の人がマッチングした人)
    OR (um.user_id = ulh.user_id AND um.to_user_id = ulh.to_user_id)
);
```

```php
select 
	um.user_id , um.to_user_id
from bridge_staging.user_matching um
where (um.user_id = 8 or um.to_user_id = 8) 
	and not exists (
		select * 
        from bridge_staging.user_block ub
		-- ブロックされた人 & ブロックした人
        where (um.user_id=ub.target_user_id and ub.user_id = um.to_user_id)  -- 自分(to_user_id)がAさんをいいねして、自分がAさんを後でブロックしている
            -- ブロックした人 & ブロックされた人
			or (um.to_user_id = ub.user_id and ub.target_user_id = um.user_id) -- 相手が自分をいいねして、相手が後で自分をブロック
		    -- ブロックした人 & ブロックされた人
            or (um.user_id=ub.user_id and ub.target_user_id = um.to_user_id)  -- 自分(to_user_id)がAさんをいいねして、相手が自分を後でブロックしている
			-- ブロックした人 & ブロックされた人
			or (um.to_user_id = ub.user_id and ub.user_id = um.user_id) -- 相手が自分をいいねして、自分が相手を後でブロック
            
	);
```

```php
select 
	u.user_status, -- user_idとto_user_idの両方のuser_statusの値を取得している
    um.user_id , um.to_user_id
from bridge_staging.user_matching um
join bridge_staging.user u
-- 結合条件
-- マッチングの片方が8でない方に合わせて合わせて
on (um.user_id = u.id and um.to_user_id = 8) or (um.to_user_id = u.id and um.user_id = 8)
where (um.user_id = 8 and u.user_status in (3,5,6,7,8,9)) or (um.to_user_id = 8 and u.user_status in (3,5,6,7,8,9))
	and not exists (
		select * 
        from bridge_staging.user_block ub
		-- ブロックされた人 & ブロックした人
        where (um.user_id=ub.target_user_id and ub.user_id = um.to_user_id)  -- 自分(to_user_id)がAさんをいいねして、自分がAさんを後でブロックしている
            -- ブロックした人 & ブロックされた人
			or (um.to_user_id = ub.user_id and ub.target_user_id = um.user_id) -- 相手が自分をいいねして、相手が後で自分をブロック
		    -- ブロックした人 & ブロックされた人
            or (um.user_id=ub.user_id and ub.target_user_id = um.to_user_id)  -- 自分(to_user_id)がAさんをいいねして、相手が自分を後でブロックしている
			-- ブロックした人 & ブロックされた人
			or (um.to_user_id = ub.user_id and ub.user_id = um.user_id) -- 相手が自分をいいねして、自分が相手を後でブロック
            
	);
```
