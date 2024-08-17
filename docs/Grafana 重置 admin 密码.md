

密码太多太多犹如天上繁星，当小本本没用的时候，你还曾会记得？


## 忘记 admin 密码重置密码


### 方法一：命令行

该方法基于 Grafana 4.1 以上

```sh
grafana-cli admin reset-admin-password <new password>
```

如果上述方法报错：Could not find config defaults, make sure homepath command line parameter is set or working directory is homepath

那么使用如下 flag 来指定参数

```sh
grafana-cli admin reset-admin-password --homepath "/usr/share/grafana" newpass
```

