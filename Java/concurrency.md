
This document talks about all the details related to the world of parallelism in Java.
We will begin with the question of what feels like parallel(Concurrency) and what is actual parallel(Tasks running on multiple cores in CPU).

What is Parallelism?
Parellelism is exeucting multiple things at once. Lets assume we have a task of cooking food where we have 2 people working together in a kitchen. 
Plan is set to make breads(rotis) and some vegetable. In order to optimse and speed up the process 2 people decided to distribute the task of setting dough to be handled by 1 person and cutting vegetables to be taken of by another.
This is true parallelism as the 2 tasks are being executed at the same time by two different people.
Similarly if an application is able to execute multiple tasks on different processing units (CPUs) at the same instance them its called parallelism or true parallelism.

What is Concurrency?
