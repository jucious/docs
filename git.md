- 命令
    - 提交
        git add -A
        将修改文件全部添加到暂存区，等待commit
        git commit -m jucious(描述)
        git pull origin master
        git push origin master
- 报错解决
    - git did not exit cleanly
    先将远程新建的空仓库 “ Clone ” 到本地，会得到一个空的与远程仓库对应的目录，然后将文件提交到master，再Push到远程仓库
    - pull报错refusing to merge unrelated histories
    git pull origin master --allow-unrelated-histories
