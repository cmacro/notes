# debian install go


Go是Google创建的一种现代开源编程语言，用于构建可靠，简单，高效的软件。 Go编写了许多流行的应用程序，例如Kubernetes，Docker，Terraform和Rancher。

在本教程中，我们将说明在Debian 10 Buster上下载并安装Go。包括将go加入PATH环境变量，验证go的安装，并创建一个go语言的helloworld示例。

## 下载版本

`wget https://go.dev/dl/go1.19.2.linux-amd64.tar.gz`

> 官网：
> https://go.dev/dl/


解压到 /usr/local/ 目录下

```sh
sudo tar -zxvf go1.19.2.linux-amd64.tar.gz -C /usr/local/
```

步骤 3. 设置 Go 环境


现在我们配置 `${PATH}` 包含 Go 的 bin 目录的环境变量 `/usr/loca/go/bin：`

```sh
echo "export PATH=/usr/local/go/bin:${PATH}" | sudo tee /etc/profile.d/go.sh
source /etc/profile.d/go.sh
```


要加载特定的配置文件并将环境加载到您当前的登录会话中：

```sh
echo "export PATH=/usr/local/go/bin:${PATH}" | sudo tee -a $HOME/.profile source
source $HOME/.profile
```


有了这个，Go 就可以开始工作了。 您可以通过显示程序的版本来检查它：


```
go version
```

## 相关问题

go 编译时需要 gcc

```
sudo apt install gcc
```
