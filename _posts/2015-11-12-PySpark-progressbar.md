---
title: What the PySpark progress bar means
updated: 2015-11-12
---

If you've used Spark you will have seen a progress bar when you submit jobs:

`[Stage 2: ============>    (2+2) / 960]`

What do the numbers on the RHS tell us?

`(2+2) / 960` : `(numCompletedTasks + numActiveTasks) / totalNumOfTasksInThisStage`



[Thanks](http://stackoverflow.com/questions/30245180/what-does-stage-39-0-2-2-mean)
