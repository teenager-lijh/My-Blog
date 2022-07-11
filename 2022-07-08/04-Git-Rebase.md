## Git Rebase

第二种合并分支的方法是 `git rebase`。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

Rebase 的优势就是可以创造更线性的提交历史，这听上去有些难以理解。如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。

咱们还是实际操作一下吧……



## 图解过程

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701143609662.png)

执行 `git rebase main` 后的结果：

这时候，`bugFix` 所指向的记录 `C3` 被复制了一份放在了 `C2` 的下边，但是提交记录 C3 并不是简简单单的复制一份，而是同时改变了它的基底。那么该怎么理解基底呢？原本 `C3` 记录是基于 `C1` 记录的基础上进行添加代码或者修改删除的。但是现在执行 `git rebase main` 之后，就相当于是说，把当前分支所使用的那一串记录的基底改为 `main` 分支所指向的基础，就相当于让最终的结果看起来像是：我们在 `main` 分支所指向的记录的基础上提交了新的记录一样，这就是基底的含义。基底就是从哪里开始分叉，就是基于谁进行增删改查提交新纪录。

在这个例子中，`bugFix` 分支所指向的记录只从 `main` 分支的 `C1` 开始分叉的，那么 `C1` 就是 `bugFix` 分支的基底，由于从分叉点 `C1`  到 `bugFix` 指向的记录的这个路径上只有 `C3` 一条记录，所以看起来显示只把 `C3` 复制并移动过去了，但是如果这个路径上有多条记录，那么就会把这一串记录全都按顺序排过去。  

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701143725799.png)

然后再切换分支到 main 上边，可以看出那个 * 星号在哪个分支上标识，就说明了 HEAD 指向了哪个分支，也就是当前正在使用的分支:

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701144018508.png)

执行`git rebase bugFix` 命令：

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701144053631.png)



## 任务

要完成此关，执行以下操作：

- 新建并切换到 `bugFix` 分支
- 提交一次
- 切换回 main 分支再提交一次
- 再次切换到 bugFix 分支，rebase 到 main 上

祝你好运！



## 任务实现过程

1. 初始状态

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701145100267.png)

2. 创建 `bugFix` 分支并切换到此分支上
   `git checkout -b bugFix` 或 `git branch bugFix; git switch bugFix` 或 `git branch bugFix; git checkout bugFix`

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701145226990.png)

3. 在 bugFix 分支提交一次记录
   `git commit`

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701145504186.png)

4. 切换到 main 分支，并提交一次记录

   `git switch main; git commit`

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701145555783.png)

5. 切换到 `bugFix` 分支，并且把 `bugFix` 给 `rebase` 到 `main` 上
   `git switch bugFix; git rebase main`
   这样看上去像是线性开发的过程，`rebase` 的过程就像是合并，但是原理有些不同

![](https://kuku-resources.oss-cn-beijing.aliyuncs.com/images/image-20220701145847906.png)



## 总结

修改`当前分支的基底`成为`另外一个分支所指向的节点`
修改当前分支的基底为 `branch_name` 分支所指向的节点： `git rebase branch_name`

