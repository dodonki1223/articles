# PostgreSQLのNULL値の挙動について

PostgreSQLのNULL値を使用した時の挙動についてのまとめです。

```
検証したPostgreSQLのバージョン
psqlバージョン：psql (PostgreSQL) 9.6.10
```

## NULL値のある項目の並び替えについて

NULL値を含む項目を「ORDER BY」句で「ASC」で指定すると**NULL値は最後に表示される**
NULL値の並びは実行してみないとわからない
※データの並びを変えるとNULL値の値の順番が変化するのでどういう順番なのかよくわからない

```sql
-- NULL値を含むデータを作成
  SELECT * 
    FROM (
                        SELECT NULL         AS animal, 0 AS no
              UNION ALL SELECT '1_ライオン' AS animal, 1 AS no
              UNION ALL SELECT NULL         AS animal, 6 AS no
              UNION ALL SELECT '4_ゴリラ'   AS animal, 5 AS no
              UNION ALL SELECT NULL         AS animal, 3 AS no
              UNION ALL SELECT '3_アリクイ' AS animal, 4 AS no
              UNION ALL SELECT ''           AS animal, 7 AS no
              UNION ALL SELECT '2_ぞう'     AS animal, 2 AS no
              UNION ALL SELECT ''           AS animal, 8 AS no
         ) AS jikken_data
ORDER BY animal ASC
;
```

**SQLの結果**

| animal     | no  | 
|:-----------|:---:|
|            | 7   |
|            | 8   |
| 1_ライオン | 1   |
| 2_ぞう     | 2   |
| 3_アリクイ | 4   |
| 4_ゴリラ   | 5   |
| NULL       | 3   |
| NULL       | 6   |
| NULL       | 0   |

NULL値を含む項目を「ORDER BY」句で「DESC」で指定すると**NULL値は最初に表示される**
NULL値の並びは実行してみないとわからない
※データの並びを変えるとNULL値の値の順番が変化するのでどういう順番なのかよくわからない

```sql
-- NULL値を含むデータを作成
  SELECT * 
    FROM (
                        SELECT NULL         AS animal, 0 AS no
              UNION ALL SELECT '1_ライオン' AS animal, 1 AS no
              UNION ALL SELECT NULL         AS animal, 6 AS no
              UNION ALL SELECT '4_ゴリラ'   AS animal, 5 AS no
              UNION ALL SELECT NULL         AS animal, 3 AS no
              UNION ALL SELECT '3_アリクイ' AS animal, 4 AS no
              UNION ALL SELECT ''           AS animal, 7 AS no
              UNION ALL SELECT '2_ぞう'     AS animal, 2 AS no
              UNION ALL SELECT ''           AS animal, 8 AS no
         ) AS jikken_data
ORDER BY animal DESC
;
```

**SQLの結果**

| animal     | no  | 
|:-----------|:---:|
| NULL       | 0   |
| NULL       | 6   |
| NULL       | 3   |
| 4_ゴリラ   | 5   |
| 3_アリクイ | 4   |
| 2_ぞう     | 2   |
| 1_ライオン | 1   |
|            | 8   |
|            | 7   |

## NULL値のある項目を条件にした時

今回は「1_ライオン」以外のデータを抽出しようとしたがNULL値のレコードは抽出されなかったため
NULL値を含む項目を「WHERE」句で指定すると**NULL値は無視され表示されない**

```sql
  SELECT * 
    FROM (
                        SELECT NULL         AS animal, 0 AS no
              UNION ALL SELECT '1_ライオン' AS animal, 1 AS no
              UNION ALL SELECT NULL         AS animal, 6 AS no
              UNION ALL SELECT '4_ゴリラ'   AS animal, 5 AS no
              UNION ALL SELECT NULL         AS animal, 3 AS no
              UNION ALL SELECT '3_アリクイ' AS animal, 4 AS no
              UNION ALL SELECT ''           AS animal, 7 AS no
              UNION ALL SELECT '2_ぞう'     AS animal, 2 AS no
              UNION ALL SELECT ''           AS animal, 8 AS no
         ) AS jikken_data
   WHERE animal <> '1_ライオン'
ORDER BY animal
;
```

**SQLの結果**

| animal     | no  | 
|:-----------|:---:|
|            | 7   |
|            | 8   |
| 2_ぞう     | 2   |
| 3_アリクイ | 4   |
| 4_ゴリラ   | 5   |

## NULL値のある項目をCOUNTした時

NULL値の項目をCOUNTした時、0件と表示されたため
NULL値を含む値をCOUNTすると**NULL値の項目はCOUNTされない**
※ただし「COUNT(*)」にするとキリンのレコードは正しく2件と表示された

```sql
  SELECT name
       , COUNT(value)
    FROM (
                        SELECT 'ゴリラ' AS name ,NULL AS value
              UNION ALL SELECT 'ゴリラ' AS name ,1    AS value
              UNION ALL SELECT 'ゴリラ' AS name ,5    AS value
              UNION ALL SELECT 'ゴリラ' AS name ,NULL AS value
              UNION ALL SELECT 'キリン' AS name ,NULL AS value
              UNION ALL SELECT 'キリン' AS name ,NULL AS value
              UNION ALL SELECT 'ゴリラ' AS name ,10   AS value
              UNION ALL SELECT 'ゴリラ' AS name ,0    AS value
         ) AS jikken_data
GROUP BY name
;
```

**SQLの結果**

| name   | count | 
|:-------|:-----:|
| ゴリラ | 4     |
| キリン | 0     |

## NULL値のある項目をSUMした時

NULL値と数値を含む項目をSUMした時、NULL値は無視された正しく計算された
NULL値のみの項目をSUMした時、NULLと計算された
NULL値を含む値をSUMすると**NULL値に関係なく正しくSUMされる**

```sql
  SELECT name
       , SUM(value)
    FROM (
                        SELECT 'ゴリラ' AS name ,NULL AS value
              UNION ALL SELECT 'ゴリラ' AS name ,1    AS value
              UNION ALL SELECT 'ゴリラ' AS name ,5    AS value
              UNION ALL SELECT 'ゴリラ' AS name ,NULL AS value
              UNION ALL SELECT 'キリン' AS name ,NULL AS value
              UNION ALL SELECT 'キリン' AS name ,NULL AS value
              UNION ALL SELECT 'ゴリラ' AS name ,10   AS value
              UNION ALL SELECT 'ゴリラ' AS name ,0    AS value
         ) AS jikken_data
GROUP BY name
;
```

**SQLの結果**

| name   | sum  | 
|:-------|:----:|
| ゴリラ | 16   |
| キリン | NULL |

## NULL値を含む値同士を文字列結合

NULL値と文字列結合した時、NULLと表示されたため
**NULL値と文字列結合した時はNULLになる**

```sql
SELECT *
  FROM (
                      SELECT 'ゴリラ' || NULL             AS name
            UNION ALL SELECT NULL || 'ゴリラ'             AS name
            UNION ALL SELECT 'ゴリラ' || NULL || 'キリン' AS name
            UNION ALL SELECT NULL || NULL                 AS name
       ) AS jikken_data
;
```

**SQLの結果**

| name  |
|:------|
| NULL  |
| NULL  |
| NULL  |
| NULL  |

## NULL値を含む値同士を加減乗除

NULL値と加減乗除した時、NULLと表示されたため
**NULL値を加減乗除した時はNULLになる**

```sql
SELECT *
  FROM (
                      SELECT 1 + NULL     AS value
            UNION ALL SELECT NULL + 2     AS value
            UNION ALL SELECT 3 + NULL + 4 AS value
            UNION ALL SELECT 1 - NULL     AS value
            UNION ALL SELECT NULL - 2     AS value
            UNION ALL SELECT 3 - NULL - 4 AS value
            UNION ALL SELECT 1 * NULL     AS value
            UNION ALL SELECT NULL * 2     AS value
            UNION ALL SELECT 3 * NULL * 4 AS value
            UNION ALL SELECT 1 / NULL     AS value
            UNION ALL SELECT NULL / 2     AS value
            UNION ALL SELECT 3 / NULL / 4 AS value
       ) AS jikken_data
;
```

**SQLの結果**

| value |
|:------|
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |
| NULL  |

## NULL値のCASE文

CASE式にNULL値が含まれているとそのNULL値の比較が出来ず「NULLだよ」が表示されないため
**CASE式でNULL値の比較を行うことはできない**
※COALESCEを使用してNULL値を変換すれば比較はできます

```sql
  SELECT CASE animal
             WHEN 'ゴリラ'       THEN 'ゴリラだよ'
             WHEN 'ライオン'     THEN 'ライオンだよ'
             WHEN ''             THEN '空文字だよ'
          -- WHEN animal IS NULL THEN 'NULLだよ'     -- この書き方だとエラーになる
          -- WHEN IS NULL        THEN 'NULLだよ'     -- この書き方だとエラーになる
             WHEN NULL           THEN 'NULLだよ'     -- ここをコメントにしても結果は同じである
             ELSE animal
          END
    FROM (
                        SELECT NULL       AS animal, 0 AS no
              UNION ALL SELECT 'ライオン' AS animal, 1 AS no
              UNION ALL SELECT NULL       AS animal, 6 AS no
              UNION ALL SELECT 'ゴリラ'   AS animal, 5 AS no
              UNION ALL SELECT NULL       AS animal, 3 AS no
              UNION ALL SELECT 'ゴリラ'   AS animal, 4 AS no
              UNION ALL SELECT ''         AS animal, 7 AS no
              UNION ALL SELECT 'キリン'   AS animal, 2 AS no
              UNION ALL SELECT ''         AS animal, 8 AS no
         ) AS jikken_data
;
```

**SQLの結果**

| animal       |
|:-------------|
| NULL         | 
| ライオンだよ | 
| NULL         | 
| ゴリラだよ   | 
| NULL         | 
| ゴリラだよ   | 
| 空文字だよ   | 
| キリン       | 
| 空文字だよ   | 
