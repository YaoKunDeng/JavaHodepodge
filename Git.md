# Git



1. 在github上创建一个仓库
2. 在本地创建一个同名文件
3. 编写代码
   1. git init 初始化仓库
   2. git add . 提交到暂存区
   3. git commit  【描述】提交到本地仓库
4. ![image-20220514175729221](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220514175729221.png)

①运行git remote add origin https://github.com/YaoKunDeng/test-demo.git

②git push -u origin main   





命令：

​	git status 查看仓库状态

​	git log  查看提交历史记录 

git log --author=YaoKunDeng 查看指定人提交的记录

git remove 【文件名】		删除指定文件

git mv 【改动前文件名】	【改动后文件名】

git checkout --【文件名】回到之前的状态

 git reset HEAD 【文件名】  撤销追踪