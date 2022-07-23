## 在提交树上移动

在接触 Git 更高级功能之前，我们有必要先学习在你项目的提交树上前后移动的几种方法。

一旦熟悉了如何在 Git 提交树上移动，你驾驭其它命令的能力也将水涨船高！



## 什么是 HEAD

我们首先看一下 “HEAD”。 HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

HEAD 通常情况下是指向分支名的（如 bugFix）。在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。

1. HEAD 可以指向分支名
2. HEAD 也可以指向一个具体的提交节点



## 图解过程

初始状态：

![image-20220701164246542](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701164246542.png)

执行命令后的结果：

![image-20220701164233180](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701164233180.png)

**分离 HEAD**

![image-20220701165657029](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701165657029.png)

`git checkout C1` 修改的是 HEAD 的指向

![image-20220701165715064](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701165715064.png)



## 任务

想完成此关，从 `bugFix` 分支中分离出 HEAD 并让其指向一个提交记录。

通过哈希值指定提交记录。每个提交记录的哈希值显示在代表提交记录的圆圈中。



## 图解过程

初始状态：

![image-20220701165920746](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701165920746.png)

`git checkout C4` 分离 HEAD 指向 C4

![image-20220701165950127](01-%E5%88%86%E7%A6%BB%20HEAD.assets/image-20220701165950127.png)



## 总结：

当前正在操作的记录就是 HEAD 所指向的记录

HEAD 可以直接指向一个记录，也可以让 HEAD 指向一个分支从而间接的指向一个记录

让 HEAD 指向记录 `record` : `git switch record_hash_id` 

让 HEAD 指向分支 `branch_name` : `git switch branch_name` 

其中 `record_hash_id` 是一条记录的 hash 值

branch_name 是分支名

