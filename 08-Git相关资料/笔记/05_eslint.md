### eslint
    js代码的检查工具
    下载: npm i eslint -D
    使用:
        生成配置文件 npx eslint --init
        检查js文件   npx eslint 目录名
        命中的规则:
            字符串必须使用单引号
            语句结尾不能有分号
            文件的最后必须要有换行
            
###  eslint结合git
    husky: 哈士奇, 为Git仓库设置钩子程序
    使用
        在仓库初始化完毕之后 再去安装哈士奇
        在package.json文件写配置
            "husky": {
                "hooks": {
                  "pre-commit": "npm run lint"   
                  //在git commit之前一定要通过npm run lint的检查
                  // 只有npm run lint不报错时 commit才能真正的运行
                }
              }           