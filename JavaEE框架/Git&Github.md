[toc]

# Git&Github

## 概念

* 功能：协同修改、数据备份、版本管理、权限控制、历史记录、分支管理

* 下载git：https://git-scm.com/

* git结构：工作区                ==>                暂存区                ==>                本地库
  (本地库)  写代码              git add           临时储存         git commit       历史版本 

* 代码托管中心（远程库）
  \> 局域网：GitLab
  \> 外网：GitHub、码云

* 本地库和远程库联系
  \> 团队内部协作
  <img src="E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314100410101.png" alt="image-20220314100410101" style="zoom:50%;" />

  \> 跨团队协作
  <img src="E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314100506122.png" alt="image-20220314100506122" style="zoom:50%;" />

## Git命令

### 基本操作

* git init：在当前目录下(当前项目下)创建 .git 文件，表示本地仓库已初始化
  ![image-20220314103937365](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314103937365.png)

* 设置签名：区分不同开发人员身份。本地库的签名和远程库的账号密码没有关系

  > 项目级别/仓库级别：当前本地库有效（信息保存位置：./.git/config 文件）
  > git config user.name <用户名>
  > git config user.email <邮箱>
  > <img src="E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314103741912.png" alt="image-20220314103741912"  />

  > 系统用户级别：当前操作系统的用户有效 （信息保存位置：~/.gitconfig 文件）
  > git config --global user.name <用户名>
  > git config --global user.email <邮箱>
  > ![image-20220314104300513](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104300513.png)

  > 优先级：项目级别 > 系统用户级别 （不允许二者都没有）

* git add \<file>：把工作区中新/更新的所有文件添加到暂存区
  ![image-20220314111056779](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314111056779.png)

* git commit -m "日志信息" \<file>：把暂存区中的文件添加到本地库
  ![image-20220314104104817](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104104817.png)

* git status：查看工作区、暂存区状态（绿色：暂存区的文件，红色：工作区的文件）
  ![image-20220314104037066](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104037066.png)

* 查看历史记录

  > git log
  > ![image-20220314104633521](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104633521.png)

  > git log --pretty=onelinegit
  > ![image-20220314104716133](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104716133.png)

  > git log --oneline
  > ![image-20220314104800442](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104800442.png)

  > git reflog：HEAD@{移动到当前版本需要的步数}
  > ![image-20220314104827691](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314104827691.png)

* 删除和找回文件

  > git reset --hard [局部索引值]：删除本地库文件
  > --hard参数：在本地库移动HEAD指针、重置暂存区和工作区
  > ![image-20220314105523045](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314105523045.png)

  > git reset --hard HEAD~n：删除暂存区文件
  > n：表示在当前位置上后退n步

* 比较同文件

  > git diff [文件名]：工作区和暂存区同文件比较
  > ![image-20220314111148641](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314111148641.png)

  > git diff [本地库历史版本] [文件名]：工作区和本地库历史版本同文件比较
  > ![image-20220314111259457](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314111259457.png)

### 分支操作

* 版本控制：多条线同时推进多个任务
  <img src="E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314112156145.png" alt="image-20220314112156145" style="zoom:50%;" />

* git branch [分支名]：创建分支
  ![image-20220314113537606](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314113537606.png)

* git branch -v：查看分支
  ![image-20220314113310584](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314113310584.png)

* git checkout [分支名]：切换分支
  ![image-20220314113339204](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314113339204.png)

* git merge [合并分支名]：合并分支
  ![image-20220314113519361](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314113519361.png)

  > 冲突解决：编辑文件、保存退出、添加到暂存区、添加到本地库（此时commit 不能带文件名）

### GitHub操作

#### 团队协作

* git remote -v：查看当前所有远程地址别名

* git remote add [别名] [远程地址]：创建新别名

* git push [别名] [分支名]：推送到远程库

  > 冲突解决：如果不是基于远程库最新版所作的修改，不能推送而是先拉取 

* git clone [远程地址/别名]：克隆远程库中的项目

* git pull [远程库地址别名] [远程分支名]：拉取项目

  > 冲突解决：按照分支冲突解决即可

#### 跨团队协作

* 协作方

1. Fork ：复制项目到自己的库中
2. 本地拉取修改并推送
3. Pull requests：发送已修改好项目的请求

* 团队方

1. Pull requests：查看请求信息
2. Files changed：审核代码
   Conversation：聊天消息
   Commits：收到提交的文件
3. Merge pull request：合并代码
   Confirm merge：填写日志信息
4. 将远程库拉取到本地

## SSH登录

* 免登录，步骤：
  1. `$ cd ~ // 进入当前用户的家目录`
  2. ``$ rm -rvf .ssh // 删除.ssh 目录`
  3. `$ ssh-keygen -t rsa -C [邮箱] // 运行命令生成.ssh 密钥目录`
  4. `$ cd .ssh // 进入.ssh 目录查看文件列表`
  5. `$ ls -lF` 
  6. `$ cat id_rsa.pub // 查看 id_rsa.pub 文件内容`
  7. `复制id_rsa.pub文件内容`
  8. `粘贴到 Github ==> 用户设置 ==> Settings ==> SSH and GPG keys ==> New SSH Key`
  9. `推送文件`

## Eclipse

* 创建分支：工程右键→Team→Switch To→New Branch
* 切换远程库分支：工程右键→Team→Switch To→Other→选择分支→Check Out→Check out as New Local Branch→创建对应远程库分支的本地库分支名→Finish
* 切换本地库分支：工程右键→Team→Switch To→选择分支
* 合并分支：工程右键→Team→Merge→选择合并的分支→OK
* 冲突解决：冲突文件→右键→Team→Merge Tool→修改→add/commit

### 团队协作

* 初始化本地库：工程右键→Team→Share Project→Git→Use or create repository in parent folder of project→选中项目→Create Repository→Finish

* Eclipse中要忽略的文件：.classpath   .project   .settings   target

  > GitHub 官网样例文件
  > https://github.com/github/gitignore
  > https://github.com/github/gitignore/blob/master/Java.gitignore

  1. 编写配置文件

  2. `~/.gitconfig` 中引入配置文件：路径中一定要使用“ /”  ，不能使用 “\ ”

     ~~~
     [core]
     	excludesfile = [配置文件地址]
     ~~~

* 推送到远程库：工程右键→Team→Remote→Push→Add All Branches Spec→Next→Finish

### 跨团队协作

* 克隆工程：Import→Projects from Git→Clone URL→粘贴URL、签名→Next→指定工程保存位置→import as general project→Convert to Maven Project

## GitFlow工作流

![image-20220314125327911](E:\软工\typora笔记\JavaEE框架\Git&Github.assets\image-20220314125327911.png)

- 主干分支 master
  主要负责管理正在运行的生产环境代码。永远保持与正在运行的生产环境完全一致。

  - 开发分支 develop
    主要负责管理正在开发过程中的代码。一般情况下应该是最新的代码。
    - 功能分支 feature
      为了不影响较短周期的开发工作，一般把中长期开发模块，会从开发分支中独立出来。 开发完成后会合并到开发分支。
  - 准生产分支（预发布分支） release
    较大的版本上线前，会从开发分支中分出准生产分支，进行最后阶段的集成测试。该版本上线后，会合并到主干分支。生产环境运行一段阶段较稳定后可以视情况删除。

- bug 修理分支 hotfix
  主要负责管理生产环境下出现的紧急修复的代码。 从主干分支分出，修理完毕并测试上线后，并回主干分支。并回后，视情况可以删除该分支。

  