
## 创建输出格式

`curl`命令的 `-w` 参数可以显示一些请求的结果，为了方便调整显示的内容，也避免请求命令过于长，可以创建一个显示格式的文件fmt.txt，内容如下：

```base
\n
Response Time for: %{url_effective}\n\n
DNS Lookup Time:\t\t%{time_namelookup}s\n
Redirection Time:\t\t%{time_redirect}s\n
Connection Time:\t\t%{time_connect}s\n
App Connection Time:\t\t%{time_appconnect}s\n
Pre-transfer Time:\t\t%{time_pretransfer}s\n
Start-transfer Time:\t\t%{time_starttransfer}s\n\n
Total Time:\t\t\t%{time_total}s\n\n
Download Speed:\t\t\t%{speed_download} B/s\n
Download Size:\t\t\t%{size_download} bytes\n

```

这个只需要在`curl`命令中加入`-w @fmt.txt`就可以打印上面定义好的内容

## 参数的含义

这个格式输出文件中的输出含义如下：

- `url_effective`： 执行完地址重定向之后的最终 URL；
- `time_namelookup`：DNS 服务器解析域名的时间，单位秒;
- `time_redirect`：重定向时间，包括到最后一次传输前的几次重定向的DNS解析，连接，预传输，传输时间，单位秒;
- `time_connect`：连接时间，从开始到建立TCP连接完成所用时间，包括前边DNS解析时间，如果需要单纯的得到连接时间，用这个time_connect时间减去前边time_namelookup时间。下面同理。
- `time_appconnect`：连接建立完成时间，如SSL/SSH等建立连接或者完成三次握手时间。
- `time_pretransfer`：从开始到准备传输的时间.
- `time_starttransfer`：开始传输时间。在client发出请求之后，Web 服务器返回数据的第一个字节所用的时间
- `time_total`：总时间，按秒计。精确到小数点后三位。
- `speed_download`：下载速度，单位字节/每秒。
- `size_download`：下载大小，单位字节。


## 使用命令


```sh
curl -s -w @fmt.txt -o /dev/null http://127.0.0.1:18020/auth?accessToken=qxqHeKItAalTrXD6
```


输出内容

```bash
root@usa-master:~# cat > testgetkey.sh
curl -s -w @fmt.txt -o /dev/null http://127.0.0.1:18020/auth?accessToken=qxqHeKItAalTrXD6
^C
root@usa-WhatsApp-master:~# chmod +x testgetkey.sh
root@usa-WhatsApp-master:~# ./testgetkey.sh

Response Time for: http://127.0.0.1:18020/auth?accessToken=qxqHeKItAalTrXD6

DNS Lookup Time:                0.000021s
Redirection Time:               0.000000s
Connection Time:                0.000072s
App Connection Time:            0.000000s
Pre-transfer Time:              0.000091s
Start-transfer Time:            0.000256s

Total Time:                     0.000290s

Download Speed:                 57000.000 B/s
Download Size:                  57 bytes
root@usa-master:~#
```

