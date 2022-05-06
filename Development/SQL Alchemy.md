---
tags: [python, concepts]
date: 2022-05-05T16:21:29+02:00
title: SQL Alchemy
---
[SQL Alchemy](https://docs.sqlalchemy.org/en/14/index.html) is an [[ORM]] written for [[Python]] (that uses system libraries, and thus expects a specific architecture).

In the case of M1 macs using rosetta, depending on the system libraries installed and their architecture, it can pause problems or show missing libs errors.

## One to one relationship
```python
class Parent(Base):
    __tablename__ = 'parent'
    id = Column(Integer, primary_key=True)

    # previously one-to-many Parent.children is now
    # one-to-one Parent.child
    child = relationship("Child", back_populates="parent", uselist=False)

class Child(Base):
    __tablename__ = 'child'
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer, ForeignKey('parent.id'))

    # many-to-one side remains, see tip below
    parent = relationship("Parent", back_populates="child")
```