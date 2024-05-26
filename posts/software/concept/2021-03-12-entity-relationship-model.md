---
layout: post
title: entity-Relationship model
description: ''
date: '2021-03-12'
categories: mindset
note: 這其實應該要歸類在 design，之後再來整理
publish:
---

### Why?

It helps to build database as correct and quick as possible and fit the demand of user, apps, business.

1.  It provides a preview of how all your tables should connect and what filed are going to be on each table.
2.  It helps to describe entities, attributes, relationships

### How?

To help explaining the logics of database. Three basic concepts: entities, attributes, relationships.

#### **example of Entity Relationship Diagram:**

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__U4rsEZn0uLeqAlNeW2paPA.png)

#### **Real World Example:**

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__E8GwUUoQb__DnJMQQ1FejAg.png)

1.  **Entity:** Anything in real world

\> weak entity means anything existing because an entity exists; for example, a car belongs to CEO in a company. If the CEO leaves a company, then this CEO car should also be eliminate from database.

2\. **Relation:** define the relationship between entities

3\. **Attribute:** the characteristic of entity

4\. **Cardinality:** define the type of relationship: One-to-One, One-to-Many, May to One, Many-to-Many

### What?

There are five steps:

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__QU3__WwsTWh0Mz7UMWsq2bA.png)

With following example:

In a market, there are a lot of people, both buyers and sellers, sellers has a lots of goods and buyers use money to buy them. Of course, the buyer can also sell what they bought. Then, the design should be as follow:

#### **Step1: Define Entities:**

we should have three entities: sellers, buyers, goods

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__GF9p3o1XIX2gxfgeivTUyQ.png)

#### **Step2: Define Relationship**

Sellers have some goods and buyers want to buy somethings.

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1____jvmrQDNhn7QPAQ55VU0IA.png)

#### **Step3: Cardinality**

This market stipulates each seller can only sell a goods and buyers can buy many goods.

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__Ark8m7LkxqXWp8bSTNXvUw.png)

#### **Step4: Identify Atrributes**

![](/Users/chenyongzhe/coding/practice_not_for_github/javascript_practice/medium-to-markdown/medium-export/posts/md_1623056197395/img/1__2apXnTEsWsrtbGSRYHQtJg.png)

### Reference

[**DBMS Tutorial: Database Management System Notes**  
_Database Management System (DBMS) is a collection of programs which enables its users to access a database, manipulate…_www.guru99.com](https://www.guru99.com/dbms-tutorial.html "https://www.guru99.com/dbms-tutorial.html")[](https://www.guru99.com/dbms-tutorial.html)