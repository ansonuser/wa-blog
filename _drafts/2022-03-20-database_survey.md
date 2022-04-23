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

PostgreSQL  |  Extensible                                              | Memory perfomance(forks a new process per connection)      |  Speed is imperative

                                                                         Read-heavy operations is less performant than MySQL



## No(Not only)SQL

- Key-value Databases

- Columnar Databases