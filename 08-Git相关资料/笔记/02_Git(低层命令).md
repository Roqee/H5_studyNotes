### 底层命令
    git对象
        git hash-object -w fileUrl : 生成一个key(hash值):val(压缩后的文件内容)键值对存到.git/objects
    tree对象
        git update-index --add --cacheinfo 100644 hash test.txt : 往暂存区添加一条记录(让git对象 对应 上文件名)存到.git/index
        git write-tree : 生成树对象存到.git/objects
    commit对象
        echo 'first commit' | git commit-tree treehash : 生成一个提交对象存到.git/objects
    对以上对象的查询
        git cat-file -p hash       : 拿对应对象的内容
        git cat-file -t hash       : 拿对应对象的类型
        
### 查看暂存区
    git ls-files -s        
        
    
        