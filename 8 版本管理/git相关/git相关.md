# 问题



### Github如何回退/回滚到某个版本

链接：

https://blog.csdn.net/jasonhongcn/article/details/1039851921.

https://blog.csdn.net/qq_33501751/article/details/104499286



有时当当前版本出现问题后我们需要回退到上一个版本或者某一个版本

1.github for windows的桌面平台

2.命令行模式



##### **1.github for windows的桌面平台**

在该平台下面，

- 先在GitHub Desktop 中找到该项目的History里面找到想要回滚的一个版本

- 然后点击右键选择revert this commit( 还原该提交 )



有如下效果：

![image-20220112151331867](git%E7%9B%B8%E5%85%B3.assets/image-20220112151331867.png)



![image-20220112153701297](git%E7%9B%B8%E5%85%B3.assets/image-20220112153701297.png)



![image-20220112153718253](git%E7%9B%B8%E5%85%B3.assets/image-20220112153718253.png)



- **是可以在desktop里面直接回滚，然后在undo commit里面 commit然后push的**







##### **2.命令行模式**

![image-20220112152026088](git%E7%9B%B8%E5%85%B3.assets/image-20220112152026088.png)

- 先查看版本日志
- 再根据版本的id回滚



![image-20220112153246704](git%E7%9B%B8%E5%85%B3.assets/image-20220112153246704.png)

![image-20220112153516258](git%E7%9B%B8%E5%85%B3.assets/image-20220112153516258.png)







**面试场景**

面试官问：你知道哪些版本管理工具，项目中用的什么版本管理工具？

答：平时学习，大作业的时候用的github或者gitee，项目中是用的云服务器搭建的私有gitlab。

问：你们怎么做分支管理的？

答：项目中我们会维护两个主要的分支，一个master分支，一个dev分支，其中会定期，或者有较大更新的时候，将dev分支merge到master分支；

平时分模块分工开发的时候，每个人会从dev分支创建一个自己的分支进行开发，然后提交merge请求，审核通过后merge到dev分支。



**总结**

- 每个人有自己的分支，开发完提交merge请求，审核通过后merge到dev分支；
- 另外，定期或者有必要的时候将dev分支merge到master分支