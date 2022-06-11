### 切换分支
    最佳实践: 每次切换分支前 当前分支一定得是干净的(已提交状态)
    坑: 
        在切换分支时 如果当前分支上有未暂存的修改(第一次) 或者 有未提交的暂存(第一次)
           分支可以切换成功  但是这种操作可能会污染其他分支
    动三个地方
        HEAD
        暂存区
        工作目录
        
### 后悔药
    工作区
        如何撤回自己在工作目录中的修改 : git checkout --filename
    暂存区
        如何何撤回自己的暂存  : git reset HEAD filename
    版本库              
        如何撤回自己的提交    : git commit --amend
            1.注释写错了,重新给用户一次机会改注释
        
### reset
    git log    :  
    git reflog : 主要是HEAD有变化 那么git reflog机会记录下来
    三部曲
        第一部： git rest --soft HEAD~  (--amend)  
            只动HEAD (带着分支一起移动)      
        第二部: git reset [--mixed]   HEAD~ 
            动HEAD   (带着分支一起移动)  
            动了暂存区
        第三部:  git reset --hard  HEAD~   
             动HEAD   (带着分支一起移动)  
             动了暂存区
             动了工作目录
             
### checkout
    git  checkout commithash   &   git reset --hard commithash         
        1.  checkout只动HEAD    --hard动HEAD而且带着分支一起走
        2.  checkout对工作目录是安全的   --hard是强制覆盖工作目录
    
    git checkout  commithash
    git checkout --filename  
          相比于git reset --hard  commitHash --filename  
          第一  第二步都没做
          只会动了工作目录
    git checkout  commithash  <file>    
          将会跳过第 1 步 
          更新暂存区 
          更新工作目录   
        
        
### 路径reset
    git reset HEAD filename     (reset 将会跳过第 1 步)    
        动了暂存区