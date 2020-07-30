---
description: 'ref: https://stackoverflow.com/a/40162246'
---

# Python APScheduler with wsgi setup

Scheduling requirements in a python application is usually handled by Celery and Celery beat. Celery has a   bit of learning curve and needs some time to setup. 

Advanced Python Scheduler \(apscheduler\) is another library which has all the capabilities of Celery provides and allows for scheduling jobs in simple, pythonic way.

When apscheduler is bundled into a webapp and served by a wsgi server, we might init apscheduler multiple times because the wsgi server will instantiate the webapp  for each of its worker. If the init for apscheduler is within the webapp, then the apscheduler gets intialized multiple times and the same job might run multiple times. 

apscheduler documentation suggest we run the scheduler separately from the webapp. Another workaround could be to configure the wsgi server to preload the app before forking them to multiple workers. This way, the init called only once and each worker is given the copy of the webapp already instantiated by the wsgi master process.

In gunicorn wsgi server, the option `--preload` flag does the above.

We also need to make sure that the jobstore is anything other than memory.

