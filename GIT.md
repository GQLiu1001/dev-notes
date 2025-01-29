# GIT

太sb了 邮箱先是1872写成了2872后面改过来后把@gmail.com写成@gamil.com了

```
git add .

git add GIT.md

git commit -m "dev-note"

git push -u origin main
```

修改单个文件也可以用 `git add 文件名` 代替 `git add .`



更改终端位置

修改Windows终端设置文件:

1. 打开Windows终端
2. `Ctrl`+`,` 打开设置
3. 找到PowerShell配置，修改"起始目录"为该路径



可以用git命令强制覆盖本地文件：

```
git fetch origin

git reset --hard origin/main
```

设置远程仓库

```
git remote add origin git@github.com:GQLiu1001/dev-notes.git
```

查看当前分支与远程分支

```
git branch -a 
```

将本地重命名为main

```
git branch -m master main 
git push -u origin main 
```

查看推送的邮箱

```
git log --pretty=format:"%h %ae"
```



更改错误的推送邮箱

```
git filter-branch -f --env-filter "
GIT_AUTHOR_EMAIL='moondust1196551872@gmail.com'
GIT_COMMITTER_EMAIL='moondust1196551872@gmail.com'
" HEAD
```

```
git push -f origin master
```

