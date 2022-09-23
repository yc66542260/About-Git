[TOC]

# Git具体用法和部分问题

## Git具体用法

### 1. Git 本地init后通过push推到远程仓库

> 参考：[Git本地init与push到远程仓库](https://blog.csdn.net/Byd_chao/article/details/82821897)

1. git删除： `rm -rf .git`，作用是删除本地git文件夹。

2. 查看git config信息： `git config --list`，里面包括了user.name和user.email信息。

3. 修改信息，例如用户名与邮箱：

   a. `git config --global user.name "name"`

   b. `git config --global user.email "email"`

4. 修改账号和密码：

   a. `git config –system –unset credential.helper`  #重置认证信息

   b. `git config –global http.emptyAuth true`  #认证清除

5. （先进入项目文件夹）通过命令 gitinit 把这个目录变成git可以管理的仓库：

   `git init` 

6. 把文件添加到本地版本库中：

   `git add .`

7. 把文件提交到仓库：

   `git commit -a -m "版本说明/更新日期"`

8. 关联到远端地址：

   `git remote add origin 你的远程库地址`

   如：

   `git remote add origin git@192.168.31.130:/srv/sample.git`

   注意：如果这一步出现错误，那么使用如下命令修改：

   `git remote rm origin`   #删除origin

   `git remote add origin git@git.github.com:yourname/demo.git`  #重新添加origin

9. 远端仓库与本地仓库进行合并（必做）

   `git pull --rebase origin master`

10. 将最新的修改从本地仓库推送到远端仓库

    注意：注意:我们第一次push的时候,加上-u参数,Git就会把本地的master分支和远程的master分支进行关联起来,我们以后的push操作就不再需要加上-u参数了

    `git push -u origin master`

11. 额外内容：复制分支并推送到远程仓库

    ```bash
    # 从master切分出dev分支，并推送到远端
    git checkout -b develop    
     
    # 切换到功能开发分支
    git checkout -b feature-[name_of_feature]  
     
    # 进行功能开发，在阶段性完成之后，将代码合并回本地的dev分支
    git checkout develop
     
    # 确认分支代码为最新
    git pull origin develop
     
    # 注意，一定要添加--no-ff 标记
    git merge --no-ff feature-[name_of_feature]
     
    # 如果确认feature分支已经不需要了可以删除
    # git branch -d feature-[name_of_feature]
     
    # 代码提交到远端分支
    git push origin develop
    ```

12. 保存进度的列表

    ```bash
    git stash  # git stash save 'message...'可以添加一些注释
    git stash pop  #恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区，并且会删除当前进度。
    git stash pop --index  #恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
    git stash pop stash@{1} #恢复指定的进度到工作区。stash_id是通过git stash list命令得到的
    ```


### 2. Git删除远程文件夹或文件

> [git删除远程文件夹或文件](https://blog.csdn.net/bananachong/article/details/126698847)

1.操作

预览将要删除的文件（如果不清楚该目录下是否存在不应该删除的文件），

加上 -n 这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览。

1. `git rm -r -n --cached` 文件/文件夹名称

2.确定无误后删除文件（本地的文件或文件夹不会被删除）

2. git rm -r --cached 文件/文件夹名称

3.提交到本地代码

3. git commit -m "提交说明"

4.推送到远程服务器

4. git push origin master

删除logs文件夹为例子：

![](./%5Cfigure%5C4.png)

## Git部分问题

1. Authetication failed for "https://........................"

    ![](./%5Cfigure%5C1.png)

   参考：这一部分可能是由于token过期导致的，解决办法是重新生成新的token，可以参考下面链接[[关于Support for password authentication was removed 报错的解决方案](https://blog.51cto.com/u_13657808/5658164?u_atoken=a45684e9-77d4-4054-afb0-3a8c3f8466a2&u_asession=010wy8Ubf6eZ7BjXRyDJaN22WU7DNIe7ajY4aMwXg45QB6DXyp9nN0eu63MeXKW6B2X0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K-Af6RNkQpZJLc8jc8BaPREhVaMhQoQvxDmSarRnKyUVWBkFo3NEHBv0PZUm6pbxQU&u_asig=05qQYhRevD29F9Yl3kQGqbE__I86TXIE2N8vOvPQEySbU2B5RGCAHDtZU6SIcDlyGMfqVa0FAwl5mevb5WrV-vkw9jZ77EaGf3P1GxtGoVsCM9MQngXAafEmvaIwxjeeJkE6EX8na4df6XzeZHozUuYp7Rk1ze6aq6WNyzaNo8A-39JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzV-LiEy8mTGwaE8JoxHl7taLztSBaNSwHP3nQ8utQlXiCNkNfzbmXGypMDlqpKRYAO3h9VXwMyh6PgyDIVSG1W_48ihTmF3uWhiYbJydy2oSNPiAmCOnVC6hPCK1rmxyar5DRbwl68DFzZK_2RItRKzMbAA4VR66HSlUVc2kX3IsmWspDxyAEEo4kbsryBKb9Q&u_aref=lNkCeCX4pINPMp5%2FgBOcjU4GGE0%3D)]，解决后，输入下面指令：

   `git remote set-url origin https://<token>@github.com/name/repo`

   ![](./%5Cfigure%5C2.png)

2. fatal: The remote end hung up unexpectedly

   参考：原因可能是因为项目推送的文件过大，此时需要修改git config文件中postBuffer文件的大小，使用下面命令：

   `git config --local http.postBuffer 52428800`

   具体看考下面链接[[Git：解决报错：fatal: The remote end hung up unexpectedly](https://blog.csdn.net/u013250071/article/details/81203900)]
   
3. 关于推代码时出现下图错误，可能是由于token不对引起的

    ![](./%5Cfigure%5C3.png)

    此时一个比较好的办法是：

    a. 更新token，拷贝；

    b. 当git需要输入密码的时候，直接把token拷贝上去就可以了。

    参考：[Git操作相关问题（pull/push/clone/）](https://blog.csdn.net/qq_43665602/article/details/126045006)
    
4. 下载好git却找不到id_rsa.pub

    > 参考：[下载好git却找不到id_rsa.pub](https://blog.csdn.net/changyana/article/details/125567861)

    由于在新电脑需要上传代码到github，新下载了git配置[ssh](https://so.csdn.net/so/search?q=ssh&spm=1001.2101.3001.7020)时却找不到id_rsa.pub，这是由于当前电脑还没有生成相关的密钥。

    a. 首先需要在当前想要上传的文件目录下打开git，一般git下载好后就可以通过右键来打开

    b. 输入命令`ssh-keygen -t rsa -b 4096 -C "邮箱"`，这里的邮箱替换成你自己的邮箱

    c. 连按三次回车，然后就可以创建成功

## Git部分参考

[1] [Git教程](https://www.runoob.com/git/git-tutorial.html)

[2] [Git简明教程](https://www.runoob.com/w3cnote/git-guide.html)

[3] [Git 五分钟教程](https://www.runoob.com/w3cnote/git-five-minutes-tutorial.html)

[4] [Git简明指南](https://www.runoob.com/manual/git-guide/)

[5] [Git远程仓库][(https://www.runoob.com/git/git-remote-repo.html)]

