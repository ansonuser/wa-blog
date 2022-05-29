---
title: Database Survey
categories: Computer
---



## S(Structured)QL

- Relational model

Type        | Advantages                                               | Disadvantage                                               |    Not to use
------------|:--------------------------------------------------------:|:----------------------------------------------------------:|------------------
SQLite      | lightweight, Portable(stored in a single file)           | Limited concurrency/No user management(serverless)         | lots of data/ high write volumes/ network access is required
 
MySQL       | Security/Speed(choosing not to implement certain feature)| Functional limitation (for speed reason)                   |  concurrency and large data volumes

PostgreSQL  |  Extensible                                              | Memory perfomance(forks a new process per connection)      |  Speed is imperative (跟我查到其他網站矛盾)

                                                                         Read-heavy operations is less performant than MySQL


## MySQL vs PostgreSQL


Items                                                          | PostgreSQL  | MySQL
---------------------------------------------------------------|:----------- |---------
ACID(Atomicity, Consistency, Isolation, Durability) Compliance確保資料更新不會再整個系統中遺失數據 |完全符合                                                   | 只有在使用InnoDB和NDB叢集儲存引擎時才符合標準

SQL相容性                                                      |佳                                                         | 較少

資料庫複製                                                     |單主機到一備用多/單主機，雙向複製，邏輯日誌串流複製，多層次複製 | 主機到一備用多/單主機，循環式複製，邏輯日誌串流複製，多層次複製
   
使用場景                                                       | 讀寫速度重要且資料需要驗證的大型系統中 | Web應用 (這邊看不出來重點是什麼，需要再查一下)

NoSQL 特徵                                                     | 支援JSON跟其他NoSQL功能，ex: XML跟HSTORE的key-value對應，支援JSON資料索引 | 支援JSON資料類型，沒有其他NoSQL功能也不知元JSON索引 


Materialized view (快取)                                       | 有 | 無

程式擴充                                                       | 支援各種語言 | 不可擴充

可延伸套件系統                                                 | 有專用的功能ex:新的資料型態，函數功能 | 無 



## No(Not only)SQL

- Key-value Databases

- Columnar Databases


## 關於索引

可以針對需要常搜尋的column name添加索引，該索引會創造一個B-tree與主表的primary key做映射，增加查詢速度，

但壞處是在操作(更新，插入，刪除)的時候，也需要對索引做動作，會降低操作的效能，另外就是空間使用增加是必然的

### 分類
- 邏輯分類


- 物理分類
1. 聚集: 頁層是雙向鍊表結構，並按照聚集索引的主鍵邏輯順序排列(物理順序與邏輯順序皆相同)

2. 非聚集: 該索引中索引的邏輯順序與磁碟上的物理存儲順序不同，一個表中可以有多個非聚集索引

若建立於 Heap，只管在最後一條數據後面插入新的數據

## 名詞解釋:

- InnoDB: MySQL和MariaDB的資料庫引擎 (由in-memort 跟on-disk的結構組成)


## ref:
- PostgreSQL vs MySQL: https://faq.postgresql.tw/postgresql-vs-mysql
- PostgreSQL vs MySQL: https://www.zhihu.com/question/20010554
- innodb: https://dev.mysql.com/doc/refman/5.7/en/innodb-architecture.html
- 索引: https://bbs.huaweicloud.com/blogs/303909
- 聚集索引:https://www.cnblogs.com/s-b-b/p/8334593.html