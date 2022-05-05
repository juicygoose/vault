---
tag: python
date: 2022-05-01T16:21:29+02:00
title: Flask
---
Flask is a light backend framework written in and for [[Python]]. I use it in the core services of Yalink

## Flask db
If you are working on multiple branches in parallel, it can happen that multiple migrations happen from the same revision. In that case, the history of the database migration is branched.

After a rebase, including the migrations that you didn't develop, the command `flask db upgrade` will not perform any operations.

Shooting the command `flask db history` allows to see what happens in details
```
808b7b911157 -> 218d727fdaea (head), empty message
9548c51ad41d -> 808b7b911157, empty message
9548c51ad41d -> 6d5ae5115e75 (head), empty message
02c29948ba83 -> 9548c51ad41d (branchpoint), empty message
e499c079b696 -> 02c29948ba83, empty message
e9795457a081 -> e499c079b696, empty message
bb67a61e6f4e -> e9795457a081, empty message
f7b6dd62d3d0 -> bb67a61e6f4e, empty message
<...... other migrations ......>
7db64ed92a7a -> cc9c87432107, empty message
feee4b5daed2 -> 7db64ed92a7a, empty message
a7000316cde7 -> feee4b5daed2, empty message
<base> -> a7000316cde7, empty message
```
Here we can see that the [[SQL Alchemy]] [[ORM]] shows a branchpoint where we have different paths afterwards (two heads).

In order to solve this two heads problem, we can tell the ORM to merge them by doing the command `flask db merge heads` . A new `flask db upgrade` will then properly do the modifications on the database with the migrations that were missing