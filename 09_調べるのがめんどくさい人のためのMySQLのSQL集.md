# 調べるのがめんどくさい人のためのMySQLのSQL集

Oracle、PostgreSQLと触ってきてMySQLにやってきました。世界は広いですね。  

ちょっとしたSQLなんだけどいちいち調べることが多い今日この頃……。  
なんかいろいろと辛い……。  

自分のためのSQL集です。随時更新していきたいと思います。  

実行環境は`5.6`を対象としています。  
そのままコピペで実行できるようにしているので実行してみて下さい！！

# MySQLの情報を確認用SQL

## MySQLのバージョンを確認する

```sql
SELECT VERSION();
```

| VERSION()  |
|:-----------|
| 5.6.10-log |

## 現在のタイムゾーンを確認する

```sql
SHOW VARIABLES LIKE '%time_zone%';
```

|  Variable_name    | Value    |
|:-----------------:|:--------:|
|  system_time_zone |  UTC     |
|  time_zone        |  SYSTEM  |

## 文字コードの確認

```sql
SHOW VARIABLES LIKE '%character\_set\_%';
```

|  Variable_name           | Value    |
|:------------------------:|:--------:|
| character_set_client     | utf8     |
| character_set_connection | utf8     |
| character_set_database	  | utf8     |
| character_set_filesystem | binary   |
| character_set_results    | utf8     |
| character_set_server     | utf8     |
| character_set_system     | utf8     |

それぞれの項目についてはこちらを參考に：[MySQL 文字コード確認 - Qiita](https://qiita.com/yukiyoshimura/items/d44a98021608c8f8a52a)

```sql
SHOW VARIABLES LIKE 'collation%';
```

|  Variable_name           | Value            |
|:------------------------:|:----------------:|
| collation_connection     | utf8_general_ci  |
| collation_database       | utf8_general_ci  |
| collation_server	       | utf8_general_ci  |

## データベースの一覧

```sql
  SELECT DISTINCT 
         table_schema AS database_name
    FROM information_schema.tables
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
ORDER BY table_schema
;
```

|  database_name |
|:--------------:|
|  hoge          |
|  fuga          |

## テーブルの一覧

```sql
  SELECT table_schema AS database_name
       , table_name   AS table_name
    FROM information_schema.tables
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
     AND table_type = 'BASE TABLE'
ORDER BY table_schema
       , table_type
       , table_name
;
```

|  database_name |  table_name  |
|:--------------:|:------------:|
|  hoge          |  hoge_hoge   |
|  hoge          |  hoge_fuga   |
|  fuga          |  fuga_fuga   |
|  fuga          |  fuga_hoge   |

## テーブルごとの文字コードの確認

```sql
  SELECT table_schema    AS database_name
       , table_name      AS table_name
       , table_collation AS character_info
    FROM information_schema.tables
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
     AND table_type = 'BASE TABLE'
ORDER BY table_schema
       , table_name
       , table_collation
;
```

|  database_name |  table_name  |  character_info   |
|:--------------:|:------------:|:-----------------:|
|  hoge          |  hoge_hoge   |  utf8_general_ci  |
|  hoge          |  hoge_fuga   |  utf8_general_ci  |
|  fuga          |  fuga_fuga   |  utf8_general_ci  |
|  fuga          |  fuga_hoge   |  utf8_general_ci  |

## テーブルごとのAUTO_INCREMENTの確認

```sql
  SELECT table_schema   AS database_name
       , table_name     AS table_name
       , auto_increment AS auto_increment
    FROM information_schema.tables
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
     AND table_type = 'BASE TABLE'
ORDER BY table_schema
       , table_name
;
```

|  database_name |  table_name  |  auto_increment   |
|:--------------:|:------------:|------------------:|
|  hoge          |  hoge_hoge   |  1                |
|  hoge          |  hoge_fuga   |  9562             |
|  fuga          |  fuga_fuga   |  133              |
|  fuga          |  fuga_hoge   |  10               |

## テーブルごとのカラム一覧

```sql
-- テーブルごとのカラム一覧
  SELECT table_schema              AS database_name
       , table_name                AS table_name
       , GROUP_CONCAT(column_name) AS column_names
    FROM information_schema.columns
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'infomation_schema')
GROUP BY table_schema
      ,  table_name  
```

|  database_name |  table_name  |  column_names            |
|:--------------:|:------------:|:------------------------:|
|  hoge          |  hoge_hoge   |  id,hoge_id,hoge         |
|  hoge          |  hoge_fuga   |  id,hoge_id,fuga_id,fuga |
|  fuga          |  fuga_fuga   |  id,fuga_id,fuga         |
|  fuga          |  fuga_hoge   |  id,fuga_id,hoge_id,hoge |

## カラム名からテーブルを検索する

```sql
-- カラム名からどのテーブルか検索する
  SELECT table_schema
       , table_name
       , column_name
    FROM information_schema.columns
   WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'infomation_schema')
     AND column_name LIKE '%検索したい項目名%'
ORDER BY table_schema
       , table_name
       , column_name
;
```

`hoge`で検索した場合

|  database_name |  table_name  |  column_name   |
|:--------------:|:------------:|:--------------:|
|  hoge          |  hoge_hoge   |  hoge_id       |
|  hoge          |  hoge_fuga   |  hoge_id       |
|  fuga          |  fuga_hoge   |  hoge_id       |

---

# テーブルのキー情報確認用SQL

## テーブルごとのプライマリーキー制約の一覧

```sql
-- テーブルごとのプライマリーキー制約の一覧
SELECT table_schema AS database_name
     , table_name   AS table_name
     , column_name  AS primary_key 
  FROM information_schema.KEY_COLUMN_USAGE 
 WHERE constraint_name = 'PRIMARY'
```

|  database_name |  table_name  |  primary_key  |
|:--------------:|:------------:|:-------------:|
|  hoge          |  hoge_hoge   | id            |
|  hoge          |  hoge_fuga   | id            |
|  fuga          |  fuga_fuga   | id            |
|  fuga          |  fuga_hoge   | id            |

## テーブルごとのユニークキー制約の一覧

```sql
-- テーブルごとのユニークキー制約の一覧
  SELECT table_schema              AS database_name
       , table_name                AS table_name
       , GROUP_CONCAT(column_name) AS unique_keys 
    FROM information_schema.KEY_COLUMN_USAGE
   WHERE position_in_unique_constraint = 1 
GROUP BY table_schema
       , table_name
;
```

|  database_name |  table_name  |  unique_keys                 |
|:--------------:|:------------:|:----------------------------:|
|  hoge          |  hoge_hoge   | fuga_id                      |
|  hoge          |  hoge_fuga   | hoge_id,fuga_id,hoge_fuga_id |
|  fuga          |  fuga_fuga   | hoge_id                      |
|  fuga          |  fuga_hoge   | fuga_id,hoge_id,fuga_hoge_id |

## テーブルごとの外部キー制約の一覧

```sql
-- テーブルごとの外部キー制約の一覧
  SELECT table_schema                                                                               AS database_name
       , table_name                                                                                 AS table_name
       , GROUP_CONCAT(CONCAT(column_name, '=', referenced_table_name, '.', referenced_column_name)) AS referenced
    FROM information_schema.KEY_COLUMN_USAGE
   WHERE referenced_table_name IS NOT NULL
GROUP BY table_schema
       , table_name
;
```

外部キーの項目がどのテーブルのどのカラムと紐付いているかわかる

|  database_name |  table_name  |  referenced                     |
|:--------------:|:------------:|:-------------------------------:|
|  hoge          |  hoge_hoge   | fuga_id=fuga.id                 |
|  hoge          |  hoge_fuga   | hoge_id=hoge.id,fuga_id=fuga.id |
|  fuga          |  fuga_fuga   | hoge_id=hoge.id                 |
|  fuga          |  fuga_hoge   | fuga_id=fuga.id,fuga_id=fuga.id |

## テーブルごとのプライマリーキー・ユニークキー・外部キー一覧

キー情報の一覧をまとめたSQLです  
全テーブルのキー情報が一括で見れるので超絶便利！！  
とりあえずキー情報が知りたかったらこれを実行すればいいかも

```sql
-- テーブルごとのプライマリーキー・ユニークキー・外部キー一覧
   SELECT table_info.*
        , primary_info.primary_key
        , unique_info.unique_keys
        , reference_info.referenced
     FROM (
               SELECT table_schema AS database_name
                    , table_name   AS table_name
                 FROM information_schema.tables
                WHERE table_type = 'BASE TABLE'
          ) AS table_info
LEFT JOIN (
                 SELECT table_schema              AS database_name
                      , table_name                AS table_name
                      , GROUP_CONCAT(column_name) AS unique_keys 
                   FROM information_schema.KEY_COLUMN_USAGE
                  WHERE position_in_unique_constraint = 1 
               GROUP BY table_schema
                      , table_name
          ) AS unique_info        
       ON table_info.database_name = unique_info.database_name
      AND table_info.table_name   = unique_info.table_name
LEFT JOIN (
                 SELECT table_schema AS database_name
                      , table_name   AS table_name
                      , column_name  AS primary_key 
                   FROM information_schema.KEY_COLUMN_USAGE
                  WHERE constraint_name = 'PRIMARY'
          ) AS primary_info
       ON table_info.database_name = primary_info.database_name
      AND table_info.table_name    = primary_info.table_name
LEFT JOIN (
                 SELECT table_schema                                                                               AS database_name
                      , table_name                                                                                 AS table_name
                      , GROUP_CONCAT(CONCAT(column_name, '=', referenced_table_name, '.', referenced_column_name)) AS referenced
                   FROM information_schema.KEY_COLUMN_USAGE
                  WHERE referenced_table_name IS NOT NULL
               GROUP BY table_schema
                      , table_name
          ) AS reference_info
       ON table_info.database_name = reference_info.database_name
      AND table_info.table_name    = reference_info.table_name
```

|  database_name |  table_name  |  primary_key  |  unique_keys                 |  referenced                     |
|:--------------:|:------------:|:-------------:|:----------------------------:|:-------------------------------:|
|  hoge          |  hoge_hoge   | id            | fuga_id                      | fuga_id=fuga.id                 |
|  hoge          |  hoge_fuga   | id            | hoge_id,fuga_id,hoge_fuga_id | hoge_id=hoge.id,fuga_id=fuga.id |
|  fuga          |  fuga_fuga   | id            | hoge_id                      | hoge_id=hoge.id                 |
|  fuga          |  fuga_hoge   | id            | fuga_id,hoge_id,fuga_hoge_id | fuga_id=fuga.id,fuga_id=fuga.id |

---

# データ容量確認用SQL

## データベースごとのサイズ表示

```sql
  SELECT table_schema                                                   AS database_name
       , CONCAT(SUM(data_length + index_length) / (1024 * 1024), ' MB') AS db_size
    FROM information_schema.tables
GROUP BY table_schema
ORDER BY table_schema
;
```

|  database_name | db_size      |
|:--------------:|-------------:|
|  hoge          |  3000.48 MB  |
|  fuga          |  90.6875 MB  |

## データベースごとのテーブル数表示

```sql
  SELECT table_schema AS database_name
       , COUNT(*)     AS table_count
    FROM information_schema.tables
   WHERE table_type = 'BASE TABLE'
GROUP BY table_schema
;
```

|  database_name | table_count  |
|:--------------:|-------------:|
|  hoge          |  59          |
|  fuga          |  22          |

## テーブルごとのサイズ表示

```sql
  SELECT table_schema                                                AS database_name
       , table_name                                                  AS table_name
       , CONCAT((data_length + index_length) / (1024 * 1024), ' MB') AS table_size
    FROM information_schema.tables
   WHERE table_type = 'BASE TABLE'
     AND table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
ORDER BY table_schema
       , (data_length + index_length) DESC
       , table_name
;
```

| database_name |  table_name | table_size     |
|:-------------:|:-----------:|---------------:|
| hoge          |  hoge_hoge  |  4964.0000 MB  |
| hoge          |  hoge_fuga  |  826.2031 MB   |
| fuga          |  fuga_fuga  |  1.5313 MB     |
| fuga          |  fuga_hoge  |  0.0469 MB     |

## テーブルごとのレコード数を確認する

```sql
  SELECT table_schema                   AS database_name
       , table_name                     AS table_name
       , table_rows                     AS table_rows
    FROM information_schema.tables AS `target`
   WHERE table_type = 'BASE TABLE'
     AND table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema')
ORDER BY table_schema
       , table_rows DESC
       , table_name
;
```

| database_name |  table_name | table_rows  |
|:-------------:|:-----------:|------------:|
| hoge          |  hoge_hoge  |  3000       |
| hoge          |  hoge_fuga  |  2500       |
| fuga          |  fuga_fuga  |  900        |
| fuga          |  fuga_hoge  |  13         |

---

#　データベース・テーブル情報なんでも確認用SQL

上の方で細かい単位の確認用SQLを紹介しましたが、めんどくさいので一括で情報を表示するためのSQLです  
**`このSQLを実行すればだいたいの情報は取得できると思います！！`** 

```sql
-- データベース、テーブルごとの件数・容量・カラム・キー情報・文字コード・AUTO_INCREMENTを一括で表示する
   SELECT table_info.table_schema    AS database_name
        , database_info.table_count  AS table_count
        , database_info.db_size      AS db_size
        , table_info.table_name      AS table_name
        , table_info.table_rows      AS table_rows
        , table_info.table_size      AS table_size
        , columns_info.column_names  AS columns
        , key_info.primary_key       AS primary_key
        , key_info.unique_keys       AS unique_keys
        , key_info.referenced        AS referenced
        , table_info.auto_increment  AS auto_increment
        , table_info.table_collation AS character_info
     FROM ( 
               -- 全テーブル情報
               SELECT * 
                    , CONCAT((data_length + index_length) / (1024 * 1024), ' MB') AS table_size
                 FROM information_schema.tables 
                WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'information_schema') 
                  AND table_type = 'BASE TABLE'
          ) AS table_info
LEFT JOIN (
               -- データベースの容量とテーブル数情報
                 SELECT table_schema                                                   AS database_name
                      , CONCAT(SUM(data_length + index_length) / (1024 * 1024), ' MB') AS db_size
                      , SUM(CASE WHEN table_type = 'BASE TABLE' THEN 1 ELSE 0 END)     AS table_count
                   FROM information_schema.tables
               GROUP BY table_schema
          ) AS database_info
       ON table_info.table_schema = database_info.database_name
LEFT JOIN (
               -- テーブルごとのカラム情報
                 SELECT table_schema              AS database_name
                      , table_name                AS table_name
                      , GROUP_CONCAT(column_name) AS column_names
                   FROM information_schema.columns
                  WHERE table_schema NOT IN ('mysql', 'perfomance_schema', 'infomation_schema')
               GROUP BY table_schema
                     ,  table_name           
          ) AS columns_info
       ON table_info.table_schema = columns_info.database_name
      AND table_info.table_name   = columns_info.table_name
LEFT JOIN (
             -- テーブルのキー情報（プライマリー、ユニーク、外部）
                 SELECT table_info.*
                      , primary_info.primary_key
                      , unique_info.unique_keys
                      , reference_info.referenced
                   FROM (
                             SELECT table_schema AS database_name
                                  , table_name   AS table_name
                               FROM information_schema.tables
                              WHERE table_type = 'BASE TABLE'
                        ) AS table_info
              LEFT JOIN (
                               SELECT table_schema              AS database_name
                                    , table_name                AS table_name
                                    , GROUP_CONCAT(column_name) AS unique_keys 
                                 FROM information_schema.KEY_COLUMN_USAGE
                                WHERE position_in_unique_constraint = 1 
                             GROUP BY table_schema
                                    , table_name
                        ) AS unique_info        
                     ON table_info.database_name = unique_info.database_name
                    AND table_info.table_name   = unique_info.table_name
              LEFT JOIN (
                               SELECT table_schema AS database_name
                                    , table_name   AS table_name
                                    , column_name  AS primary_key 
                                 FROM information_schema.KEY_COLUMN_USAGE
                                WHERE constraint_name = 'PRIMARY'
                        ) AS primary_info
                     ON table_info.database_name = primary_info.database_name
                    AND table_info.table_name    = primary_info.table_name
              LEFT JOIN (
                               SELECT table_schema                                                                               AS database_name
                                    , table_name                                                                                 AS table_name
                                    , GROUP_CONCAT(CONCAT(column_name, '=', referenced_table_name, '.', referenced_column_name)) AS referenced
                                 FROM information_schema.KEY_COLUMN_USAGE
                                WHERE referenced_table_name IS NOT NULL
                             GROUP BY table_schema
                                    , table_name
                        ) AS reference_info
                     ON table_info.database_name = reference_info.database_name
                    AND table_info.table_name    = reference_info.table_name
          ) AS key_info
       ON table_info.table_schema = key_info.database_name
      AND table_info.table_name   = key_info.table_name
 ORDER BY table_info.table_schema
        , table_info.table_name
;
```

|  database_name | table_count  | db_size      |  table_name  | table_rows  | table_size     |  column_names            |  primary_key  |  unique_keys                 |  referenced                     |  auto_increment   |  character_info   |
|:--------------:|-------------:|-------------:|:------------:|------------:|---------------:|:------------------------:|:-------------:|:----------------------------:|:-------------------------------:|------------------:|:-----------------:|
|  hoge          |  59          |  3000.48 MB  |  hoge_hoge   |  3000       |  4964.0000 MB  |  id,hoge_id,hoge         | id            | fuga_id                      | fuga_id=fuga.id                 |  1                |  utf8_general_ci  |
|  hoge          |  59          |  3000.48 MB  |  hoge_fuga   |  2500       |  826.2031 MB   |  id,hoge_id,fuga_id,fuga | id            | hoge_id,fuga_id,hoge_fuga_id | hoge_id=hoge.id,fuga_id=fuga.id |  9562             |  utf8_general_ci  |
|  fuga          |  22          |  90.6875 MB  |  fuga_fuga   |  900        |  1.5313 MB     |  id,fuga_id,fuga         | id            | hoge_id                      | hoge_id=hoge.id                 |  133              |  utf8_general_ci  |
|  fuga          |  22          |  90.6875 MB  |  fuga_hoge   |  13         |  0.0469 MB     |  id,fuga_id,hoge_id,hoge | id            | fuga_id,hoge_id,fuga_hoge_id | fuga_id=fuga.id,fuga_id=fuga.id |  10               |  utf8_general_ci  |

---

# 権限関連のSQL

## ユーザーごとの権限一覧

```sql
-- ユーザーごとの権限一覧
  SELECT DISTINCT 
         grantee                      AS user
       , is_grantable                 AS is_grantable   -- GRANT権限があるかどうか
       , GROUP_CONCAT(privilege_type) AS privileges
    FROM information_schema.user_privileges
GROUP BY grantee
       , is_grantable
ORDER BY grantee
;
```

| user           | is_grantable | privileges                                                        |
|:--------------:|:------------:|:------------------------------------------------------------------|
|  Administrator |  YES         |  CREATE,LOCK TABLES,EVENT,REFERENCES,CREATE VIEW,DELETE,CREATE... |
|  hoge          |  NO          |  CREATE,DELETE,ALTER,UPDATE,INDEX,INSERT,DROP,SELECT              |
|  fuga          |  NO          |  SELECT                                                           |

## データベースごとの権限一覧

```sql
-- データベースごとの権限一覧
  SELECT grantee                      AS user
       , table_schema                 AS db
       , GROUP_CONCAT(privilege_type) AS privileges 
    FROM information_schema.schema_privileges 
GROUP BY grantee
       , table_schema
ORDER BY grantee
       , table_schema
;
```

## テーブルごとの権限一覧

```sql
-- テーブルごとの権限一覧
  SELECT grantee                      AS user
       , table_schema                 AS db
       , table_name                   AS `table`
       , GROUP_CONCAT(privilege_type) AS privileges 
    FROM information_schema.table_privileges  
GROUP BY grantee
       , table_schema
       , table_name
ORDER BY grantee
       , table_schema
       , table_name
;
```

| user  | db     | table      | privileges    |
|:-----:|:------:|:-----------|:--------------|
|  hoge |  hoge  |  hoge_hoge | SELECT        |
|  fuga |  fuga  |  fuga_fuga | SELECT,UPDATE |

## カラムごとの権限一覧

```sql
-- カラムごとの権限一覧
  SELECT grantee                      AS user
       , table_schema                 AS db
       , table_name                   AS `table`
       , column_name                  AS `column`
       , GROUP_CONCAT(privilege_type) AS privileges 
    FROM information_schema.column_privileges 
GROUP BY grantee
       , table_schema
       , table_name
       , column_name
ORDER BY grantee
       , table_schema
       , table_name
       , column_name
;
```

| user  | db     | table      |  column    | privileges |
|:-----:|:------:|:-----------|:-----------|:-----------|
|  hoge |  hoge  |  hoge_hoge |  hoge_name | UPDATE     |
|  fuga |  fuga  |  fuga_fuga |  fuga_name | UPDATE     |

## ユーザー、データベース、テーブル、カラムごとの権限一覧

全権限が見れて超絶便利！！

參考URL：[MySQL権限一覧をきれいに作る方法と、rootユーザー以外で棚卸しする方法 - Qiita](https://qiita.com/yoshi-taka/items/94467014ec7e9e3e8ec8#%E4%B8%80%E6%8B%AC%E3%81%A7%E3%83%AA%E3%82%B9%E3%83%88%E3%81%AB%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E3%81%AA%E3%82%89)

```sql
-- ユーザー、データベース、テーブル、カラムごとの権限一覧
  SELECT *
    FROM (
                        SELECT grantee AS user, is_grantable AS is_grantable, '-'          AS db, '-'        AS `table`, '-'         AS `column`, GROUP_CONCAT(privilege_type) AS privileges FROM information_schema.user_privileges   GROUP BY grantee, is_grantable
              UNION ALL SELECT grantee AS user, is_grantable AS is_grantable, table_schema AS db, '-'        AS `table`, '-'         AS `column`, GROUP_CONCAT(privilege_type) AS privileges FROM information_schema.schema_privileges GROUP BY grantee, is_grantable, table_schema
              UNION ALL SELECT grantee AS user, is_grantable AS is_grantable, table_schema AS db, table_name AS `table`, '-'         AS `column`, GROUP_CONCAT(privilege_type) AS privileges FROM information_schema.table_privileges  GROUP BY grantee, is_grantable, table_schema, table_name
              UNION ALL SELECT grantee AS user, is_grantable AS is_grantable, table_schema AS db, table_name AS `table`, column_name AS `column`, GROUP_CONCAT(privilege_type) AS privileges FROM information_schema.column_privileges GROUP BY grantee, is_grantable, table_schema, table_name, column_name
         ) AS authority
ORDER BY user
       , db
       , `table`
       , `column`
;
```

---

# 日付関連のSQL

## タイムゾーンを変換して現在時刻を取得

`UTC`から`Asia/Tokyo`の現在時刻を取得

```sql
SELECT NOW()                                  AS utc
     , CONVERT_TZ(NOW(), 'UTC', 'Asia/Tokyo') AS asia_tokyo
;
```

|  utc                |  asia_tokyo          |
|:-------------------:|:--------------------:|
| 2019-07-06 02:56:43 | 2019-07-06 11:56:43  |

## Date型を好きな形式に変換する

```sql
-- 9999-99-99形式に日付を変換する
SELECT @a := DATE_FORMAT(NOW(), '%Y-%m-%d');
-- 99:99:99形式に日付を変換する
SELECT @b := DATE_FORMAT(NOW(), '%H:%i:%S');
-- （曜日）の形式に日付を変換する
SELECT @c := CASE DATE_FORMAT(NOW(), '%w')
                 WHEN 0 THEN '（日）'
                 WHEN 1 THEN '（月）'
                 WHEN 2 THEN '（火）'
                 WHEN 3 THEN '（水）'
                 WHEN 4 THEN '（木）'
                 WHEN 5 THEN '（金）'
                 WHEN 6 THEN '（土）'
              END;
-- 「9999-99-99 99:99:99（曜日）」の形式
SELECT CONCAT(@a, ' ', @b, @c) AS result;
```

| result                    |
|:-------------------------:|
| 2019-07-06 02:54:35（土） |

下記の指定子を使用することができます

| 指定子 | 説明                                                             |
|:------:|:-----------------------------------------------------------------|
| %a     |  簡略曜日名 (Sun..Sat)                                           |
| %b     |  簡略月名 (Jan..Dec)                                             |
| %c     |  月、数字 (0..12)                                                |
| %D     |  英語のサフィクスを持つ日付 (0th, 1st, 2nd, 3rd, …)             |
| %d     |  日、数字 (00..31)                                               |
| %e     |  日、数字 (0..31)                                                |
| %f     |  マイクロ秒 (000000..999999)                                     |
| %H     |  時間 (00..23)                                                   |
| %h     |  時間 (01..12)                                                   |
| %I     |  時間 (01..12)                                                   |
| %i     |  分、数字 (00..59)                                               |
| %j     |  年間通算日 (001..366)                                           |
| %k     |  時 (0..23)                                                      |
| %l     |  時 (1..12)                                                      |
| %M     |  月名 (January..December)                                        |
| %m     |  月、数字 (00..12)                                               |
| %p     |  AM または PM                                                    |
| %r     |  時間、12 時間単位 (hh:mm:ss に AM または PM が続く)             |
| %S     |  秒 (00..59)                                                     |
| %s     |  秒 (00..59)                                                     |
| %T     |  時間、24 時間単位 (hh:mm:ss)                                    |
| %U     |  週 (00..53)、日曜日が週の初日、WEEK() モード 0                  |
| %u     |  週 (00..53)、月曜日が週の初日、WEEK() モード 1                  |
| %V     |  週 (01..53)、日曜日が週の初日、WEEK() モード 2、%X とともに使用 |
| %v     |  週 (01..53)、月曜日が週の初日、WEEK() モード 3、%x とともに使用 |
| %W     |  曜日名 (Sunday..Saturday)                                       |
| %w     |  曜日 (0=Sunday..6=Saturday)                                     |
| %X     |  年間の週、日曜日が週の初日、数字、4 桁、%V とともに使用         |
| %x     |  年間の週、月曜日が週の初日、数字、4 桁、%v とともに使用         |
| %Y     |  年、数字、4 桁                                                  |
| %y     |  年、数字 (2 桁)                                                 |
| %%     |  リテラル 「%」 文字                                             |
| %x     |  x (上記にないすべての 「x」)                                    |

[MySQL :: MySQL 5.6 リファレンスマニュアル :: 12.7 日付および時間関数より](https://dev.mysql.com/doc/refman/5.6/ja/date-and-time-functions.html)

## 今日、昨日、明日

```sql
SELECT @now       := NOW();
SELECT @yesterday := (NOW() - INTERVAL 1 DAY);
SELECT @tomorrow  := (NOW() + INTERVAL 1 DAY);

SELECT @now       AS 今日
     , @yesterday AS 昨日
     , @tomorrow  AS 明日
```

| 今日                |  昨日               | 明日                |
|:-------------------:|:-------------------:|:-------------------:|
| 2019-06-27 00:41:41 | 2019-06-26 00:41:41 | 2019-06-28 00:41:41 |

## １週間前、１週間後

```sql
SELECT @own_week_ago    := (NOW() - INTERVAL 7 DAY);
SELECT @own_week_before := (NOW() + INTERVAL 7 DAY);

SELECT @own_week_ago    AS １週間前
     , @own_week_before AS １週間後
```

| １週間前            |  １週間後           |
|:-------------------:|:-------------------:|
| 2019-06-20 00:41:41 | 2019-07-04 00:41:41 |

## 先月、来月、３ヶ月後、３ヶ月末

```sql
SELECT @last_month         := DATE_FORMAT((NOW() - INTERVAL 1 MONTH), '%Y-%m-%d');
SELECT @next_month         := DATE_FORMAT((NOW() + INTERVAL 1 MONTH), '%Y-%m-%d');
SELECT @three_month_before := DATE_FORMAT((NOW() - INTERVAL 3 MONTH), '%Y-%m-%d');
SELECT @three_month_ago    := DATE_FORMAT((NOW() + INTERVAL 3 MONTH), '%Y-%m-%d');

SELECT @last_month         AS 先月
     , @next_month         AS 来月
     , @three_month_before AS ３ヶ月前
     , @three_month_ago    AS ３ヶ月後
```

| 来月        |  先月       |  ３ヶ月前    |  ３ヶ月後    |
|:-----------:|:-----------:|:------------:|:------------:|
| 2019-06-27  | 2019-05-27  | 2019-03-27   | 2019-09-27   |

## 今月の月初と月末

```sql
SELECT @begin_month := DATE_FORMAT(NOW(), '%Y-%m-01');
SELECT @end_month   := LAST_DAY(NOW());

SELECT @begin_month AS 今月の月初
     , @end_month   AS 今月の月末
```

| 今月の月初  |  今月の月末  |
|:-----------:|:------------:|
| 2019-06-01  | 2019-06-30   |

## 先月の月初と月末、来月の月初と月末

```sql
-- [先月の月初]今月の月末＋１から２ヶ月前を取得し求める
SELECT @last_month_start := DATE_ADD(DATE_ADD(LAST_DAY(NOW()), INTERVAL 1 DAY), INTERVAL -2 MONTH);
-- [先月の月末]今月の月末＋１から１ヶ月前から−１日を取得し求める
SELECT @last_month_end   := DATE_ADD(DATE_ADD(DATE_ADD(LAST_DAY(NOW()), INTERVAL 1 DAY), INTERVAL -1 MONTH),INTERVAL -1 DAY);
-- [来月の月初]現在日付の月末を求めた後＋１日を取得し求める
SELECT @next_month_start := DATE_ADD(LAST_DAY(NOW()), INTERVAL 1 DAY);
-- [来月の月末]現在日付の１ヶ月後の最終日を取得し求める
SELECT @next_month_end := LAST_DAY(DATE_ADD(NOW(), INTERVAL 1 MONTH));

SELECT @last_month_start AS 先月の月初
     , @last_month_end   AS 先月の月末
     , @next_month_start AS 来月の月初
     , @next_month_end   AS 来月の月末
```

| 先月の月初  |  先月の月末  |  来月の月初  |  来月の月末  |
|:-----------:|:------------:|:------------:|:------------:|
| 2019-05-01  | 2019-05-31   | 2019-07-01   | 2019-07-31   |

## 去年、来年、３年前、３年後

```sql
SELECT @last_year         := DATE_FORMAT((NOW() - INTERVAL 1 YEAR), '%Y-%m-%d');
SELECT @next_year         := DATE_FORMAT((NOW() + INTERVAL 1 YEAR), '%Y-%m-%d');
SELECT @three_year_before := DATE_FORMAT((NOW() - INTERVAL 3 YEAR), '%Y-%m-%d');
SELECT @three_year_ago    := DATE_FORMAT((NOW() + INTERVAL 3 YEAR), '%Y-%m-%d');

SELECT @last_year         AS 去年
     , @next_year         AS 来年
     , @three_year_before AS ３年前
     , @three_year_ago    AS ３年後
```

| 去年        |  来年       |  ３年前      |  ３年後      |
|:-----------:|:-----------:|:------------:|:------------:|
| 2018-06-27  | 2020-06-27  | 2016-06-27   | 2022-06-27   |

---

# NULL値変換

こちらにわかりやすく書かれていました  
[mysqlのifnullとcoalesceの違いは何ですか？ - コードログ](https://codeday.me/jp/qa/20190117/151837.html)

NULL値の比較対象の数に応じて使い分けるのが良いかも:thinking:

## IFNULL

引数を２つ取り、１つ目の値がNULLの時は２つ目の引数の値を返す

```sql
SELECT IFNULL(NULL, 1), IFNULL(1, 2)
;
```

|  IFNULL(NULL, 1) |  IFNULL(1, 2)  |
|:----------------:|:--------------:|
|  1               |  1             |

## COALESCE

２つ以上のパラメータを取り、最初の非NULL値を返す。非NULL値がない場合は、NULLを返す  
１つはパラメータをNULL以外にしないと変換ができない……

```sql
SELECT COALESCE(NULL, 1)          
     , COALESCE(NULL, 2, NULL)    
     , COALESCE(NULL, 2, 3)       
     , COALESCE(NULL, NULL, NULL) 
;
```

|  COALESCE(NULL, 1) |  COALESCE(NULL, 2, NULL)  |  COALESCE(NULL, 2, 3) | COALESCE(NULL, NULL, NULL) |
|:------------------:|:-------------------------:|:---------------------:|:--------------------------:|
|  1                 |  2                        |  2                    |   NULL                     |

---

# Tips集

## 数値を３桁区切りにする

```sql
SELECT FORMAT(999999999999, 0) AS digit_12
     , FORMAT(999999999, 0)    AS digit_9
     , FORMAT(999999, 0)       AS digit_6
     , FORMAT(999, 0)          AS digit_3
     , FORMAT(9, 0)            AS digit_1
```

|  digit_12         |  digit_9        |  digit_6          |  digit_3          |  digit_1          |
|:-----------------:|:---------------:|:-----------------:|:-----------------:|:-----------------:|
|  999,999,999,999  |  999,999,999    |  999,999          |  999              |  9                |

## 好きなデータの表を作成する

何かSQLの構文を試したい時は下記のような感じで記述するとテーブルが無くても好きなデータの表を取得することができます  
`これを使うとSELECTの結果でINSERTする時に便利です（入れるデータをSELECT文で確認できるし修正が楽ちん）  `

```sql
-- 好きなデータの表を作成する
SELECT *
  FROM (
                      SELECT 'ゴリラ'     AS animal, '地上' AS habitat
            UNION ALL SELECT 'キリン'     AS animal, '地上' AS habitat
            UNION ALL SELECT 'カバ'       AS animal, '水上' AS habitat
            UNION ALL SELECT 'ワニ'       AS animal, '水上' AS habitat
            UNION ALL SELECT 'マントヒヒ' AS animal, '地上' AS habitat
       ) AS animals
```

|  habitat  |  animal     |
|:---------:|:-----------:|
|  地上     |  ゴリラ     |
|  地上     |  キリン     |
|  水上     |  カバ       |
|  水上     |  ワニ       |
|  地上     |  マントヒヒ |

## 連番を振る

`ROW_NUMBER`関数が無いので変数を使用して再現する

```sql
-- 連番を振る
SET @row_number = 0;
  SELECT @row_number := @row_number + 1 AS `row`
       , animal
    FROM (
                        SELECT 'ゴリラ'     AS animal
              UNION ALL SELECT 'キリン'     AS animal
              UNION ALL SELECT 'カバ'       AS animal
              UNION ALL SELECT 'ワニ'       AS animal
              UNION ALL SELECT 'マントヒヒ' AS animal
         ) AS animals
ORDER BY animal
```

| row |  animal     |
|----:|:-----------:|
| 1   |  カバ       |
| 2   |  キリン     |
| 3   |  ゴリラ     |
| 4   |  マントヒヒ |
| 5   |  ワニ       |

## グループごと連番を振る

`WINDOW関数`が無いので仕方なく下記のやり方で再現する  
`WINDOW関数`使いたいよー・ﾟ・(つД｀)・ﾟ・　　
**`新しいバージョンはWINDOW関数があるので注意`**

參考サイト：[[SQL]mysqlで同項目毎に連番をつける | 目黒で働く分析担当の作業メモ](http://avespluton.jp/lab/blog/tec/sqlmysql%E3%81%A7%E5%90%8C%E9%A0%85%E7%9B%AE%E6%AF%8E%E3%81%AB%E9%80%A3%E7%95%AA%E3%82%92%E3%81%A4%E3%81%91%E3%82%8B)  
これにさらに条件を追加することでグループごと５件取得するなどが可能になる

```sql
-- グループごとの連番を振る
  SELECT @row_num := IF(@prev_value = habitat, @row_num + 1, 1) AS no
       , habitat                                                AS habitat
       , animal                                                 AS animal
       , @prev_value := habitat                                 AS prev_value
    FROM (
                        SELECT 'ゴリラ'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'キリン'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'カバ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'ワニ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'マントヒヒ' AS animal, '地上' AS habitat
         ) AS animals
       , ( SELECT @row_num    := 1  ) AS ROW_DATA
       , ( SELECT @prev_value := '' ) AS PREV_DATA
ORDER BY habitat
       , animal
```

| no  |  habitat  |  animal     |  prev_value  |
|----:|:---------:|:-----------:|:------------:|
| 1   |  地上     |  キリン     |  地上        |
| 2   |  地上     |  ゴリラ     |  地上        |
| 3   |  地上     |  マントヒヒ |  地上        |
| 1   |  水上     |  カバ       |  水上        |
| 2   |  水上     |  ワニ       |  水上        |

## 項目の値ごとカウントを取る

`CASE`を使用しカウント対象かどうかを判断し`SUM`を使用することで項目の値ごとカウントを取ることが出来る

```sql
-- 項目の値ごとカウントを取る
  SELECT COUNT(*)                                        AS all_count
       , SUM(CASE habitat WHEN '地上' THEN 1 ELSE 0 END) AS chijou_count
       , SUM(CASE habitat WHEN '水上' THEN 1 ELSE 0 END) AS suijou_count
    FROM (
                        SELECT 'ゴリラ'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'キリン'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'カバ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'ワニ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'マントヒヒ' AS animal, '地上' AS habitat
         ) AS animals
```

|  all_count  |  chijou_count  |  suijou_count  |
|:-----------:|:--------------:|:--------------:|
|     5       |        3       |        2       |

## 特定の値を全レコードに付加する

１レコードしか無いテーブルのデータを付加したい時に使ったりします  
そもそも`CROSS JOIN`はこんなことぐらいでしか使用したことない……  

結合先のデータが２レコード以上あると`結合元 * 結合先`の数のレコードが出来上がってしまう

```sql
-- 特定の値を全レコードに付加する
    SELECT *
      FROM (
                          SELECT 'ゴリラ'     AS animal, '地上' AS habitat
                UNION ALL SELECT 'キリン'     AS animal, '地上' AS habitat
                UNION ALL SELECT 'カバ'       AS animal, '水上' AS habitat
                UNION ALL SELECT 'ワニ'       AS animal, '水上' AS habitat
                UNION ALL SELECT 'マントヒヒ' AS animal, '地上' AS habitat
           ) AS animals
CROSS JOIN (SELECT 'なんか付加するぞ' AS add_data) AS add_data
;
```

|  habitat  |  animal     |  add_data          |
|:---------:|:-----------:|:------------------:|
|  地上     |  ゴリラ     |  なんか付加するぞ  |
|  地上     |  キリン     |  なんか付加するぞ  |
|  水上     |  カバ       |  なんか付加するぞ  |
|  水上     |  ワニ       |  なんか付加するぞ  |
|  地上     |  マントヒヒ |  なんか付加するぞ  |

## 特定の項目の一覧を１レコードで取得する

`GROUP_CONCAT`を使用することで特定の項目の一覧を１レコードに収めた状態で取得できます

```sql
  SELECT habitat
       , GROUP_CONCAT(animal) AS animal_list
    FROM (
                        SELECT 'ゴリラ'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'キリン'     AS animal, '地上' AS habitat
              UNION ALL SELECT 'カバ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'ワニ'       AS animal, '水上' AS habitat
              UNION ALL SELECT 'マントヒヒ' AS animal, '地上' AS habitat
         ) AS animals
GROUP BY habitat
;
```

|  habitat  |  animal_list              |
|:---------:|:-------------------------:|
|  地上     |  ゴリラ,キリン,マントヒヒ |
|  水上     |  カバ,ワニ                |

---

# その他

## トランザクションを使用してデータを更新

詳しくは[公式のドキュメント](https://dev.mysql.com/doc/refman/5.6/ja/commit.html)を参照すること

```sql
-- トランザクションを開始
START TRANSACTION;

-- データを更新
UPDATE hogehoge SET hoge = 'ゴリラ' WHERE id = 1;
UPDATE hogehoge SET hoge = 'キリン' WHERE id = 2;

-- コミット
-- 更新が失敗しているようだったらROLLBACK
COMMIT;
```

## テーブルのロック状態を確認する

それぞれの項目の意味は[公式ドキュメント](https://dev.mysql.com/doc/refman/5.6/ja/innodb-locks-table.html)を参照すること  
**ストレージエンジンが`InnoDB`の場合**

```sql
SELECT * FROM information_schema.innodb_locks;
```

## SQLの実行計画を確認する

なんかこのSQL遅くないって時は実行計画をとりあえずみますよね  
EXPLAINの後にSELECT文などを記述すると確認できる

```sql
EXPLAIN SELECT * FROM information_schema.tables;
```

詳しくは下記サイトを參考

- [MySQL :: MySQL 5.6 リファレンスマニュアル :: 13.8.2 EXPLAIN 構文](https://dev.mysql.com/doc/refman/5.6/ja/explain.html)
- [漢(オトコ)のコンピュータ道: MySQLのEXPLAINを徹底解説!!](http://nippondanji.blogspot.com/2009/03/mysqlexplain.html)
- [MySQLでのSQLチューニングについて（EXPLAINの見方） | Opentone Labs.](http://labs.opentone.co.jp/?p=1985)

---

# 最後に

最近はFrameworkを使用しているとO/Rマッパー君が勝手にSQLを発行してくれるので自分で書くことが少なくなりました。  
たまにはSQLを書きたいんじゃー！！
