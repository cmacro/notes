# 日志自动拆分

服务部署多了日志一大堆，占用大量硬盘。没事就可能撑爆服务器硬盘。

解决之道：压缩日志，并清除当前日志。保留N天历史日志,并每天调用一次


## 需要个压缩日志脚本 

创建 `logbak.sh`, 备份 nohup 产生的日志 `sample.log` 

```sh
#!/bin/bash
DIR=/var/log/sample/
d=`date +%Y%m%d%H`
cd $DIR
cp sample.log ${d}sample.log
echo "" > sample.log
tar -czf ${d}sample.log.tar.gz ${d}sample.log
rm ${d}sample.log
find /var/log/sample/ -type f -mtime +10 -name "*.log.tar.gz" -exec rm -rf {} \;
```


## 启动定时任务

```
crontab -e

## 添加内容
1 0 * * * sh /opt/sample/logbak.sh

```


重启 crontab服务

```
service cron restart
```

### 定时说明 

```
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 6) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

使用者也可以将所有的设定先存放在文件中，用 crontab file 的方式来设定执行时间。

| 执行时间                 | 格式       |
| ----------------------- | ---------- |
| 每分钟定时执行一次         | * * * * * |
| 每小时定时执行一次         | 0 * * * * |
| 每天定时执行一次           | 0 0 * * * |
| 每周定时执行一次           | 0 0 * * 0 |
| 每月定时执行一次           | 0 0 1 * * |
| 每月最后一天定时执行一次    | 0 0 L * * |
| 每年定时执行一次           | 0 0 1 1 * |


其他： 查看任务列表 

```
crontab -l
```