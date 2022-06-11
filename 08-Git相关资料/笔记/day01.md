.git/objects/06/e21bb0105e2de6c846725a9a7172f57dd1af96   workspae项目的第一个版本(树对象)
.git/objects/56/0a3d89bf36ea10794402f6664740c284d4ae3b   test.txt文件的第一个版本(git对象)

.git/objects/9d/74ec4055e0f1edc1921d749c250380ca7b5ebd   workspae项目的第二个版本(树对象)
.git/objects/c3/1fb1e89d8b6b3ef34cdb5a2f999d6e29b822ba   test.txt文件的第二个版本(git对象)
.git/objects/ea/e614245cc5faa121ed130b4eba7f9afbcc7cd9   new.txt文件的第一个版本(git对象)

git操作最基本的流程
    创建工作目录 对工作目录进行修改
    git add ./
        git hash-object -w 文件名(修改了多少个工作目录中的文件 此命令就要被执行多少次)
        git update-index ...
    git commit -m "注释内容"
        git write-tree
        git commit-tree
        
git高层命令(CRUD)
    git init            初始化仓库
    git status          查看文件的状态
    git diff            查看哪些修改还没有暂存
    git diff --staged   查看哪些修改以及被暂存了 还没提交
    git log --oneline   查看提交的历史记录
    git add ./          将修改添加到暂存区
    git rm 文件名       删除工作目录中对应的文件 再将修改添加到暂存区
    git mv 原文件名 新文件名  将工作目录中的文件进行重命名 再将修改添加到暂存区
    git commit 
    git commit -a 
    git commit -a -m 注释  
                    将暂存区提交到版本库       

git高层命令(分支)
    git branch                显示分支列表
    git branch 分支名         创建分支
    git checkout 分支名       切换分支
    git branch -D 分支名      强制删除分支
    


