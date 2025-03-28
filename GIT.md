# GIT

一键运行111222

```bash
git add . && git commit -m "dev-note" && git push -u origin main
```

设置邮箱

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱地址"
git config --global --list
```

```bash
git add GIT.md

git add .

git commit -m "dev-note"

git push -u origin main
```

修改单个文件也可以用 `git add 文件名` 代替 `git add .`

11111

更改终端位置

修改Windows终端设置文件:

1. 打开Windows终端
2. `Ctrl`+`,` 打开设置
3. 找到PowerShell配置，修改"起始目录"为该路径



可以用git命令强制覆盖本地文件：

```bash
git fetch origin

git reset --hard origin/main
```

设置远程仓库

```bash
git remote add origin git@github.com:GQLiu1001/dev-notes.git
```

查看当前分支与远程分支

```bash
git branch -a 
```

将本地重命名为main

```bash
git branch -m master main 
git push -u origin main 
```

查看推送的邮箱

```bash
git log --pretty=format:"%h %ae"
```



更改错误的推送邮箱

```bash
git filter-branch -f --env-filter "
GIT_AUTHOR_EMAIL='moondust1196551872@gmail.com'
GIT_COMMITTER_EMAIL='moondust1196551872@gmail.com'
" HEAD
```
强制推送
```bash
git push -f origin master
```



22端口报错

在.ssh目录下新建一个config没后缀

复制以下内容 更改为443接口

```

Host github.com
  Hostname ssh.github.com
  Port 443

```

Obisidian

![{859F1ABE-061B-42DC-A5C7-3EC9BD44C6BB}.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/img/%7B859F1ABE-061B-42DC-A5C7-3EC9BD44C6BB%7D.png)
![{34E6CC6A-1824-4335-80E7-E31CF4072C17}.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/img/%7B34E6CC6A-1824-4335-80E7-E31CF4072C17%7D.png)


vscode443错误
找到vpn代理端口 7897
设置
```bash
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
git config --global -l

ipconfig /flushdns
```
