# 清除Git无效证书

电脑重启后git拉去库时出现账号异常情况。


## debian 处理

linux下账号失效错误信息，掉电之类会出现

```sh
error: object file .git/objects/89/aeb5f0c14058844a41ac452852862247d8c022 is empty
fatal: loose object 31aeb5f0c14058844a41ac452852862247d8c022 (stored in .git/objects/98/aeb5f0c14058844a41ac452852862247d8c022) is corrupt
```

暴力清除

```sh
find .git/objects/ -type f -empty | xargs rm
git fetch -p
git fsck --full
```

> 原帖：https://stackoverflow.com/questions/11706215/how-can-i-fix-the-git-error-object-file-is-empty



## windows 处理


windows下报错信息

```sh
git.exe pull --progress -v --no-rebase "origin"
fatal: An error occurred while sending the request.
fatal: The underlying connection was closed: An unexpected error occurred on a receive.
fatal: Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host.
fatal: An existing connection was forcibly closed by the remote host
...
```

处理方法：重置账号 

设置清空git保存的账号密码并每次操作时强制输入账号密码，即git不自定保存账号密码

```sh
git config --system --unset credential.helper

git credential-manager uninstall
git config --global credential.helper wincred

查看config配置：git config --list
查看git用户名：git config user.name
清除缓存的用户名和密码：git credential-manager uninstall
更改全局用户名：git config --global user.name "username"
更改全局邮箱：git config --global user.email "eamil@xx.com"
```

设置git自动保存输入的账号密码

```sh
git config --global credential.helper store
```

