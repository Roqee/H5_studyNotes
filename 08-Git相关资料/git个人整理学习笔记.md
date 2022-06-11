# Git 学习

## 基本概念

### 版本控制的分类

#### 集中式（svn）

svn 因为每次存的都是差异，需要的磁盘空间会相对小一点，可是回滚的速度会很慢。

优点：代码存放在单一的服务器上，便于项目的管理。

缺点：

- 服务器宕机：员工写的代码得不到保障。
- 服务器炸了：整个项目的历史记录都会丢失。

#### 分布式（git） 

git 每次存的都是项目的完整快照，需要的代码会相对的大一点

​	（Git 项目团队对对代码做了极致的压缩，最终需要的实际空间比svn多不了太多，可是git的回滚速度极快）

优点：

- 完全的分布式

缺点：

- 学习起来比SVN陡峭



## git 使用

### 底层命令

git(blob) 对象底层命令：

- git hash-object -w fileUrl：生成一个 key(hash值): value(压缩后的文件内容) 存到 .git/objects

tree 对象：

- git updata-index --add --cacheinfo 10644(文件类型) hash test.txt：往暂存区添加一条记录（让 git 对象对应上文件名）存到 .git/index
- git write tree：生成树对象存到 .git/objects

提交对象：

- echo 'first commit' | git commit-tree treehash：生成一个提交对象存到 .git/objects

对以上对象的查询：

- git  cat-file -p hash：拿对应 git 对象的内容
- git cat-file -t hash：拿对应对象的类型

#### 查看暂存区命令

git ls-files -s

#### 三个区域

git本质上是一个数据库（存代码），流程：工作区 ——》 暂存区——》版本库

- 工作区：沙箱环境，git不会进行管理，随便修改。
  - 执行git init之后，所属路径就是工作区了
- 暂存区：一些小的修改，都可以放在暂存区
  - .git 里的 index 文件
- 版本库：所有修改完成之后，提交生成一个版本（提交了一系列的修改（暂存区里））
  - .git 里面的 object 文件夹就是版本库

#### 三个对象

- Git 对象
  - key: value 组成的键值对（key 是 value 对应的 hash）
  - 键值对在 git 内部是一个blob类型的对象
- 树对象
- 提交对象

**项目的版本就是一个提交对象，本质上项目的快照就是一个树对象**

总结：

- 一个完整的流程最少要包含一个 git 对象、一个树对象、一个提交对象。
- 一次提交必定只会有一个树对象、一个提交对象，git 对象可以有很多



### git 操作最基本的流程

1. 创建工作目录，对工作目录进行修改

   + `git add ./`  相当于执行了以下底层命令：
     + git hash-object -w 文件名称：生成git对象放到git 版本库（修改了多少个工作目录中的文件，此命令就被执行多少次）
     + git update-index ... ：把git版本库中的git对象放入暂存区
   + git commit -m '注释内容'
     + git write-tree：把暂存区的git对象生成树对象
     + git commit-tree hash：把树对象包装成提交对象 

   

### 高层命令（CRUD）

#### 安装

傻瓜式安装：下一步下一步。

- git --version：查看版本信息

#### 初始化配置

- git config --global user.name "damu"
- git config --global user.email "damu$example.com"

#### 初始化仓库

- git init

#### C（新增）

在工作目录中新增文件

- git status
- git add ./ 
- git commit -m "msg"

#### U（修改）

在工作目录中修改文件

- git status
- git add ./
- git commit -m "msg"

#### D（删除 & 重命名）

- git rm 要删除的文件			git mv 要修改的文件名  新文件名
- git status                               git status
- git commit -m "msg"           git commit -m "msg"

#### R（查询）

- git cat-file -p HEAD：查看当前提交对象
- git cat-file -p master^{tree}（或者是树对象的 hash)：查看当前提交对象对应的树对象

- git status：查看工作目录中文件的状态（已跟踪（已提交、已暂存、已修改）、未跟踪）
- git diff：查看未暂存的修改
- git diff  --staged：查看未提交的暂存
- git log --oneline：查看未提交的历史记录



### 高层命令总结

- git init 			                 初始化仓库 
- git status                         查看文件的状态
- git diff                              查看哪些修改还没有暂存
- git diff --staged               查看哪些修改已经被暂存了还没提交
- git log --oneline              查看提交的历史记录（一行显示并取前几位hash）
- git log                               查看提交的历史记录
- git reflog                          查看所有提交的历史记录（删除的也会显示出来，只要动HEAD就会记住）
- git log --pretty=oneline    查看提交的历史记录（一行显示，hash显示完全）
- git log --oneline --decorate --graph --all            查看项目分叉历史
- git add                             将修改添加到暂存区

```shell
# 先将工作目录里面的修改做成git对象放到版本库，再从版本库中把git对象拿出来放到暂存区里面
# 先到版本库，再到暂存区
git add ./
```

- git rm 文件名                           删除工作目录中对应的文件，再将修改添加到暂存区
- git mv 原文件名 新文件名       将工作目录中的文件进行重命名，再将修改添加到暂存区
- git commit                               注释比较多的时候用
- git commit -a                           跳过暂存区直接提交（已经被跟踪过的才可以使用 -a ）
- git commit -a -m                     注释比较少的时候用
- git commit -m '注释'		  	将暂存区提交到版本库生成提交对象



### 高层命令（分支）

#### git 分支本质

- 分支本质是一个提交对象，分支都会被 HEAD 引用（HEAD一个时刻只会指向要给分支）
- 当我们有新的提交的时候，HEAD 会携带当前持有的分支往前移动

#### git 分支命令

- 创建分支：git branch 分支名

- 切换分支：git checkout 分支名

- 创建&切换分支：git checkout -b 分支名
- **版本穿梭（时光机）：git branch 分支名 commitHash**：在任意提交对象上创建分支
- 普通删除分支：git branch -d 分支名（当前分支已合并）
- 强制删除分支：git branch -D 分支名
- 合并分支：git merge 分支名（一般回到主分支进行合并）
  - 快进合并：不会产生冲突
  - 典型合并：有机会产生冲突
  - 解决冲突：打开冲突的文件进行修改，修改完后重新 add 、commit



- 查看分支列表：git branch
- 查看合并到当前分支的分支列表：git branch --merged
  - 一旦出现在这个列表中，就应该删除
- 查看没有合并到当前分支的分支列表：git branck --no-merged
  - 一旦出现在这个列表中，就应该观察以下是否要合并

#### 分支的注意点

- ==在切换的时候一定要保证当前分支是干净的！！！！！==

  - 允许切换分支：
    1. 分支上所有的内容处于 已提交状态
    2. 分支上的内容是初始化创建，处于未跟踪状态（避免）
    3. 分支上的内容是初始化创建，第一次处于已暂存状态（避免）
  - 不允许切分支：分支上的内容处于已修改状态 或 第二次以后的已暂存状态

  

在分支上的工作做到一半时，如果有切换分支的需求，我们应该将现有的工作存储起来

- git stash：会将当前分支上的工作推到一个栈中（有做一次提交，但是git log不记录，git reflog 会记录）
- 然后进行分支切换，进行其他工作，完成其他工作后切回原分支
- git stash apply：将栈顶的工作内容还原，但不让任何内容出栈
- git stash drop：取出栈顶的工作内容后，就应该将其删除（出栈）
- git stash pop：git stash apply + git stash drop
- git stash list：查看存储



HEAD：

1. 是一个指针，它默认指向 master 分支，切换分支时就是让HEAD指向不同的分支
2. 每次有新的提交时，HEAD都会带着当前指向的分支，一起往前移动

分支相关命令：

- git log --oneline --decorate --graph --all：查看整个项目的分支图

- git branch：查看分支列表
- git branch -v：查看分支指向的最新的提交

- git branch 分支名：在当前提交对象上创建新的分支
- git branch 分支名 commithash：在指定的提交对象上创建新的分支（时光机）
- git checkout 分支名：切换分支
- git checkout -b 分支名：在当前提交对象上创建一个分支并切换到该分支
- git branch -d：删除空的分支，删除已经被合并的分支
- git branch -D：强制删除分支

#### 分支切换注意事项：

- 分支切换会改变你工作目录中的文件
- 在切换分支时，一定要注意你工作目录里的文件会被改变。如果是切换到一个比较旧的分支，你的工作目录恢复到分支最后一次提交时的样子。如果 git 不能干净利落地完成这个任务，他将禁止切换分支。
- ==每次在切换分支前，提交一下当前分支==

#### 切换分支动三个地方：

**最佳实践：每次切换分支前，当前分支一定是干净的（已提交状态）**

**坑：在切换分支时，如果当前分支上有未暂存的修改（第一次，后续的修改不会）或者有未提交的暂存（第一次，后续的修改不会）**

​		**分支可以切换成功，但是这种操作可能会污染其他分支**

1. HEAD
2. 暂存区
3. 工作目录



### git 后悔药

git 后悔药的原理就对应着 reset 三部曲

1. 撤销工作目录的修改——对应 --hard
   + 如何撤回自己在工作目录中的修改：git checkout -- filename  或者   git restore filename

2. 撤销暂存区的修改——对应 --mixed
   + 如何撤回自己的暂存：git reset HEAD filename  或者  git restore --staged filename

3. 撤销提交（版本库）——对应 --soft
   + 如何撤回自己的提交
     1. 注释写错了，重新给用户一次机会改注释
   + git add forgotten_filename 然后 git commit --amend



### reset

git log：查看提交的历史记录

git reflog：只要是HEAD有变化，那么 git reflog 就会记录下来

#### reset 三部曲：

- 第一部：`git reset --soft HEAD~`或`git reset --soft commithash`(比较像 git commit --amend)
  - 只动 HEAD（带着分支一起移动）
  - git reset --soft commithash：用commithash 的内容重置HEAD内容
  - HEAD~：当前HEAD的前一个提交对象
- 第二部：`git reset [--mixed] HEAD~` 或 git reset [--mixed] commithash
  - 动 HEAD（带着分支一起移动）
  - 动了暂存区
  - git reset [--mixed] commithash：用 commithash 的内容重置HEAD内容、重置暂存区
- 第三部：`git reset --hard HEAD~`(checkout 的底层命令，只不过做了一个优化，checkout 分支没有跟着一起走) 或 git reset --hard commithash
  - 动 HEAD（带着分支一起移动）
  - 动了暂存区
  - 动了工作目录
  - git reset --hard commithash：用commithash的内容重置HEAD内容、重置暂存区、重置工作目录

#### 路径 reset

git reset [--mixed] HEAD~ filename  或 git reset [--mixed] commithash filename （reset 将会跳过第 1 步）

- 用 commithash 中 filename 的内容区重置暂存区

- 所有的路径 reset 都要省略第一步！！！
  - 第一步是重置 HEAD 内容，我们知道HEAD本质指向一个分支，分支的本质是一个提交对象
  - 提交对象指向一个树对象，树对象又很有可能指向多个 git 对象，一个 git 对象代表一个文件！！！！
  - HEAD 可以代表一系列文件的状态！！！



#### checkout 深入理解

git checkout commithash 和 git reset --hard commithash 特别像

- 共同点

  - 都需要重置 HEAD、暂存区、工作目录

- 区别

  1. checkout 动 HEAD 时不会带着分支走，而是切换分支；--hard 动 HEAD，而且带着分支一起走

  2. checkout 对工作目录是安全的，--hard 是强制覆盖工作目录

checkout + 路径：

- **git checkout commithash filename**
  - 跳过第一步（更新HEAD）
  - 重置暂存区
  - 重置工作目录

- **git checkout -- filename**
  - 重置工作目录



### eslint

js 代码的检查工具

下载：

```shell
npm i eslint -D
```

使用：

1. 生成配置文件：npx eslint --init
2. 检查 js 文件：npx eslint 目录名
3. 命中的规则：
   + 字符串必须使用单引号
   + 语句结尾不能有分号
   + 文件最后要有换行

#### eslint 结合 git

husky：哈士奇，为 Git 仓库设置钩子程序

使用：

- 在仓库初始化完毕之后，再去安装哈士奇

- 在 package.json 文件中写配置

  ```json
  "husky": {
    "hooks": {
      "pre-commit": "num run lint" 
      // 在 git commit 之前一定要通过 npm rum lint 检查
      // 只有 npm run lint 不报错时 commit 才能真正的yu
    }
  }
  ```

### 三个分支

#### 本地分支

- 正常的数据推送  和 拉取步骤

  1. ==确保本地分支已经跟踪了远程跟踪分支：git branch -u 远程跟踪分支名（别名/分支名)==

  2. 拉取数据：git pull

  3. 上传数据：git push
- 一个本地分支怎么去跟踪一个远程跟踪分支

  1. 当克隆的时候，会自动生成一个 master本地分支（已经跟踪了对应的远程跟踪分支）
2. 在新建其他分支时，可以指定想要跟踪的远程跟踪分支
  
   * git checkout -b 本地分支名 远程跟踪分支名(remote/分支名)
     * git checkout --track 远程跟踪分支名（创建跟远程跟踪分支同名的本地分支）
  3. 将一个已经存在的本地分支 改成 一个跟踪分支（跟踪远程跟踪分支的分支）

     * git branch -u 远程跟踪分支名


#### 远程跟踪分支（remote/分支名）

#### 远程分支



### 团队协作

1. 项目经理初始化远程仓库

   + 一定要初始化一个空的仓库：在 github 上操作

2. 项目经理创建待推送的本地仓库

   1. git remote 别名 仓库地址（https）
   2. git init；将源码复制进来
   3. 修改用户名、修改邮箱
   4. git add
   5. git commit

3. 项目经理推送本地仓库到远程仓库

   1. 清理 windows 凭据
   2. git push 别名 分支（输入用户名、密码；推完之后会附带生成远程跟踪分支）

4. 项目经理邀请成员 & 成员接受邀请

   + 在 github 上操作

   

5. 成员克隆远程仓库

   1. git clone 仓库地址（在本地生成 .git 文件，默认位本地仓库配了别名 origin。默认主分支有对应的远程跟踪分支）
      * 只有在克隆的时候，本地分支master 和 远程跟踪分支别名/master 是有同步关系的

6. 成员做出贡献

   1. 成员新建分支进行工作：git branch -b 新建个人分支名
   2. 修改源码文件
   3. git add
   4. git commit 
   5. git push 别名 分支（输入用户名、密码；推完之后会附带生成远程跟踪分支）

7. 项目经理更新修改 

   1. git fetch 别名（将修改同步到远程跟踪分支上）：全量拉取（把仓库的分支全部拉取下来）
   2. 创建仓库里对应远程跟踪分支的本地分支：git branch 分支名（对应远程跟踪分支）
   3. 切换到创建的分支上：git checkout 分支名（新创建的分支名）
   4. 合并：git merge 远程跟踪分支



### 冲突

git 本地操作会不会有冲突？

- 典型合并的时候

git 远程协作的时候会不会有冲突？

- push
- pull



### 远程协作的基本流程

- 第一步：项目经理创建一个空的远程仓库
- 第二步：项目经理创建一个待推送的本地仓库
- 第三步：为远程仓库配别名，配完用户名、邮箱
- 第四步：在本地仓库中初始化代码，提交代码
- 第五步：推送
- 第六步：邀请成员
- 第七步：成员克隆远程仓库
- 第八步：成员做出修改
- 第九步：成员推送自己的修改
- 第十步：项目经理拉取成员的修改



### 做跟踪

克隆仓库是会自动为 master 做跟踪

两种情况：

1. 本地没有分支
   - git checkout --track 远程跟踪分支（remote/分支名）：会自动创建一个分支名一样的本地跟踪分支，而且已经建立好了跟踪关系

2. 本地已经创建了分支
   + git branch -u 远程跟踪分支（remote/分支名）

推送和拉取前都要做跟踪

###　推送

- git push

### 拉取

- git pull

### pull request

- 让第三方人员参与到项目中 fork

### 使用频率最高的五个命令

1. git status
2. git add
3. git commit
4. git push
5. git pull



### SSH

- ssh -keygen -t rsa -C 你的邮箱：生成公私钥

- .ssh 文件位置：C:\Users\Administrator\\.ssh

- ssh -T git@github.com：测试公私钥是否已经配对













