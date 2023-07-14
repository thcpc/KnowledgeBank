#java  #多线程 #threadlocal #InheritableThreadLocal

> JDK 原文 
>this class extends ThreadLocal to provide inheritance of values from parent thread to child thread: when a child thread is created, the child receives initial values for all inheritable thread-local variables for which the parent has values. Normally the child's values will be identical to the parent's; however, the child's value can be made an arbitrary function of the parent's by overriding the childValue method in this class. inheritable thread-local variables are used in preference to ordinary thread-local variables when the per-thread-attribute being maintained in the variable (e.g., User ID, Transaction ID) must be automatically transmitted to any child threads that are created.


# 引用文章
[InheritableThreadLocal详解](https://blog.csdn.net/yexiguafu/article/details/103900568)
