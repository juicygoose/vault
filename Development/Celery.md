---
tag: python
date: 2022-05-01T16:21:29+02:00
title: Celery
---
# Intro
Celery can be used as an async framework for [[Python]]. It uses a broker (database). Most of the time we use [redis](https://docs.celeryq.dev/en/stable/getting-started/backends-and-brokers/redis.html) as a broker. 
# Periodic tasks
[Periodic tasks](https://docs.celeryq.dev/en/stable/userguide/periodic-tasks.html) can be setup in celery to execute tasks. We can then use them to send notifications from the backend based on specific filters. 
## Adding periodic tasks
We need to add the tasks to something called the beat schedule list

```python
from celery import Celery
from celery.schedules import crontab

app = Celery()

@app.on_after_configure.connect
def setup_periodic_tasks(sender, **kwargs):
    # Calls test('hello') every 10 seconds.
    sender.add_periodic_task(10.0, test.s('hello'), name='add every 10')

    # Calls test('world') every 30 seconds
    sender.add_periodic_task(30.0, test.s('world'), expires=10)

    # Executes every Monday morning at 7:30 a.m.
    sender.add_periodic_task(
        crontab(hour=7, minute=30, day_of_week=1),
        test.s('Happy Mondays!'),
    )

@app.task
def test(arg):
    print(arg)

@app.task
def add(x, y):
    z = x + y
    print(z)
```
