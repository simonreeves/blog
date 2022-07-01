---
layout: post
title: "Deadline Command"
tags:
  - deadline
  - python
---

# Deadline Webservice
## Submit job via python (Deadline Connect)
This is using the deadline [webservice](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/manual/web-service.html) via DeadlineConnect


[Good example of manual deadline job submission via python is the Analog cache maya plugin](https://github.com/analogstudio/Analog_Maya/blob/master/2019/modules/Analog/scripts/an_cache.py)



```python
def create_deadline_connection():
    # connect to deadline webservice via API
    deadline_con = Deadline.DeadlineConnect.DeadlineCon('192.168.1.99', 8081)

    # Test if connecting was successful
    try:
        deadline_con.Groups.GetGroupNames()
    except:
        print('Could not connect to deadline webservice, is pulse running?')
        return False

    return deadline_con

```

# Deadline Command

https://docs.thinkboxsoftware.com/products/deadline/10.1/2_Scripting%20Reference/class_deadline_1_1_scripting_1_1_repository_utils.html#a5659d8ca562137c802a729cd2e9666a4

## Test python code
It's useful to test python code that is intended for custom events etc. without reloading a live job.



windows git bash example
```bash
$ '/c/Program Files/Thinkbox/Deadline10/bin/deadlinecommand.exe' executescript deadline_test.py
```
then you can test code that would work 'inside' deadline like an event.

```python
import pprint
import Deadline.Scripting

def __main__():
    jobs = Deadline.Scripting.RepositoryUtils.GetJobs(invalidateCache=True)

    for job in jobs:       
        job_status = job.JobStatus

        if job_status.lower() != 'active':
            continue

        print(job_status)
        pprint.pprint(job.JobName)
        pprint.pprint(job.JobSendJobErrorWarning)
        
```