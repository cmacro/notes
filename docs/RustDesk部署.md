# RustDesk部署

doc
https://rustdesk.com/docs/zh-cn/self-host/rustdesk-server-oss/install/


现在，我们就可以进行RustDesk的部署。如果你不是使用创建的用户进行登录，那么你可以使用`su`命令进行切换：

|   |   |
|---|---|
|1  <br>2|```<br># 切换到mintimate用户sudo su - mintimate<br>```|

为了方便管理，我们创建一个目录，便于管理：

|   |   |
|---|---|
|1  <br>2|```<br># 创建一个空目录，用于安装RustDeskmkdir ~/myApplication<br>```|

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%B8%8B%E8%BD%BD%E6%9C%8D%E5%8A%A1%E7%AB%AF)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%B8%8B%E8%BD%BD%E6%9C%8D%E5%8A%A1%E7%AB%AF "下载服务端")下载服务端

进入RustDesk的Github地址：

- [RustDesk Server Github 发布地址](https://github.com/rustdesk/rustdesk-server/releases)

我们使用的是X86版本服务器，所以这里我们下载X86版本的服务包：  
[![下载X86版本的服务端](https://imagehost.mintimate.cn/post_guideToHostRustDesk/copyDownloadServerLink.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/copyDownloadServerLink.webp "下载X86版本的服务端")

下载X86版本的服务端

拷贝下载地址，到服务器上使用wget进行下载和解压：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6|```<br># 使用wget进行下载1.1.8-2版本（最新版本可以看上述发布地址）wget https://github.com/rustdesk/rustdesk-server/releases/download/1.1.8-2/rustdesk-server-linux-amd64.zip# 使用unzip解压unzip rustdesk-server-linux-amd64.zip# 重命名解压后文件（方便管理）mv amd64 RustDesk<br>```|

[![下载解压并重命名](https://imagehost.mintimate.cn/post_guideToHostRustDesk/downloadAndUnzipRustDeskFinish.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/downloadAndUnzipRustDeskFinish.webp "下载解压并重命名")

下载解压并重命名

现在，我们的RustDesk前期的准备就完成了。接下来，我们尝试运行，之后再注册为服务。

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E5%B0%9D%E8%AF%95%E8%BF%90%E8%A1%8C)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E5%B0%9D%E8%AF%95%E8%BF%90%E8%A1%8C "尝试运行")尝试运行

我们可以先尝试运行一下；确保可以使用后，我们再使用`pm2`挂起访问，或者`systemctl`注册为服务。

为了方便我们测试，这里使用`screen`双开两个虚拟终端，并以前台模式挂起后端进程：

|   |   |
|---|---|
|1  <br>2|```<br># 安装screensudo apt install screen<br>```|

之后，在刚刚我们的RustDesk解压后的目录内，可以看到相关的文件：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4|```<br>RustDesk├── hbbr├── hbbs└── rustdesk-utils<br>```|

[![RustDesk Server内包含的内容](https://imagehost.mintimate.cn/post_guideToHostRustDesk/includeFilesInRustDeskServer.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/includeFilesInRustDeskServer.webp "RustDesk Server内包含的内容")

RustDesk Server内包含的内容

解释一下各个作用：

- hbbs: RustDesk的ID服务，用于分配和注册ID；
- hbbr: RustDesk的中继服务，主要远程访问就是这个，如果直连远程不行，会使用hbbr进行流量中继。

之后，我们使用两个screen进行启动：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4|```<br># 创建一个叫myHbbs的虚拟终端：screen -R myHbbs# 运行hbbs./hbbs<br>```|

通常情况下就会启动成功：  
[![使用screen启动hbbs](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenHostHbbs.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenHostHbbs.webp "使用screen启动hbbs")

使用screen启动hbbs

紧接着，`Ctrl+a`和`d`返回主终端，启动**hbbr**。

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4|```<br># 创建一个叫yHbb的虚拟终端：screen -R myHbbr# 运行hbbr./hbbr<br>```|

到此，如果没什么防火墙、安全组问题，RustDesk就启动完成了。  
[![使用screen启动hbbr](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenHostHbbr.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenHostHbbr.webp "使用screen启动hbbr")

使用screen启动hbbr

[![使用screen启动完成](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenLsOfRustDesk.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/screenLsOfRustDesk.webp "使用screen启动完成")

使用screen启动完成

查看这个**RustDesk**目录，就可以发现，多了一些数据库文件和一个证书文件：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8  <br>9|```<br>RustDesk├── db_v2.sqlite3├── db_v2.sqlite3-shm├── db_v2.sqlite3-wal├── hbbr├── hbbs├── id_ed25519├── id_ed25519.pub└── rustdesk-utils<br>```|

我们需要拷贝`.pub`这个公钥文件，用于接下来的**本地配置**：  
[![拷贝公钥文件](https://imagehost.mintimate.cn/post_guideToHostRustDesk/autoPubFile.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/autoPubFile.webp "拷贝公钥文件")

拷贝公钥文件

比如，我的公钥文件内容：`C6bJn7*******************50nCK3y4=`

接下来，我们可以进行本地的配置。

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE "本地配置")本地配置

到RustDesk上的客户端下载地址，下载我们的客户端：

- [RustDesk 客户端 Github 发布地址](https://github.com/rustdesk/rustdesk/releases)

通常情况下，我们下载最新的发布版本即可：  
[![下载最新版本客户端](https://imagehost.mintimate.cn/post_guideToHostRustDesk/downloadRustDeskClient.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/downloadRustDeskClient.webp "下载最新版本客户端")

下载最新版本客户端

比如： 我这里安装macOS，并打开：  
[![在macOS上打开RustDesk](https://imagehost.mintimate.cn/post_guideToHostRustDesk/openRustDeskClient_macOS.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/openRustDeskClient_macOS.webp "在macOS上打开RustDesk")

在macOS上打开RustDesk

点击上面的三个点，进入配置：  
[![进入RustDesk的配置界面](https://imagehost.mintimate.cn/post_guideToHostRustDesk/enterConfigMacOS.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/enterConfigMacOS.webp "进入RustDesk的配置界面")

进入RustDesk的配置界面

找到网络配置：  
[![网络配置界面](https://imagehost.mintimate.cn/post_guideToHostRustDesk/configNetWork.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/configNetWork.webp "网络配置界面")

网络配置界面

解锁网络配置，上方两个输入你的服务器IP，下方的Key输入上一步内我们拷贝的公钥（对于我是`C6bJn7*******************50nCK3y4=`），如果你有使用域名，IP部分也可以使用域名（CDN不行）：  
[![配置我们自己的服务器](https://imagehost.mintimate.cn/post_guideToHostRustDesk/configWithServerConfig.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/configWithServerConfig.webp "配置我们自己的服务器")

配置我们自己的服务器

> 我们没有开启强制加密，所以理论上key不填写，也可以进行连接；只是，不填写key，网络传输可能不安全。

到此，我们的配置配置就完成了。我们可以体验一下。

# [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%B5%8B%E8%AF%95%E4%BD%BF%E7%94%A8)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%B5%8B%E8%AF%95%E4%BD%BF%E7%94%A8 "测试使用")测试使用

现在，我使用一台Linux虚拟机和我的macOS进行演示，首先在Kali Linux上安装RustDesk：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4|```<br># 下载最新版本的RustDesk Debian客户端wget https://github.com/rustdesk/rustdesk/releases/download/1.2.2/rustdesk-1.2.2-aarch64.deb# 使用dpkg进行安装sudo dpkg -i rustdesk-1.2.2-aarch64.deb<br>```|

[![Linux上安装RustDesk](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installFinishRustDeskLinux.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installFinishRustDeskLinux.webp "Linux上安装RustDesk")

Linux上安装RustDesk

安装好后，打开RustDesk：  
[![Linux上打开RustDesk](https://imagehost.mintimate.cn/post_guideToHostRustDesk/openRustDeskLinux.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/openRustDeskLinux.webp "Linux上打开RustDesk")

Linux上打开RustDesk

和刚刚一样，我们需要配置好我们自己的服务器参数。在macOS上就可以远程它了：  
[![macOS上发起远程请求](https://imagehost.mintimate.cn/post_guideToHostRustDesk/askLinuxWithRustDesk.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/askLinuxWithRustDesk.webp "macOS上发起远程请求")

macOS上发起远程请求

这个时候，实际上Linux上也是可以直接点击接受的：  
[![Linux上接受远程请求](https://imagehost.mintimate.cn/post_guideToHostRustDesk/acceptWithRustDesk.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/acceptWithRustDesk.webp "Linux上接受远程请求")

Linux上接受远程请求

最后的“套娃”效果：  
[![远程协助的效果](https://imagehost.mintimate.cn/post_guideToHostRustDesk/FinishDemo.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/FinishDemo.webp "远程协助的效果")

远程协助的效果

# [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%B3%A8%E5%86%8C%E6%9C%8D%E5%8A%A1)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E6%B3%A8%E5%86%8C%E6%9C%8D%E5%8A%A1 "注册服务")注册服务

现在，我们回到服务端。刚刚使用的`screen`可能不太优雅，这里我们准备了两个方案注册为访问，方便管理。

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%BD%BF%E7%94%A8pm2%E8%BF%90%E8%A1%8C)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%BD%BF%E7%94%A8pm2%E8%BF%90%E8%A1%8C "使用pm2运行")使用pm2运行

如果要使用pm2运行，需要提前安装nodejs，使用nodejs的软件包管理器（npm或者yarn）安装pm2。

安装nodejs的方法，我在之前的文章内已经说的十分详细，这里就不再过多介绍，如果不知道如何操作，可以参考：

- [Windows/macOS/Linux上安装Node.js，并使用NVM管理多版本Node.js](https://www.mintimate.cn/2021/07/26/nvmNode)

就简单说一下思路和使用命令吧：

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8  <br>9  <br>10  <br>11|```<br># 安装nvmgit clone https://github.com/nvm-sh/nvm.git ~/.nvm# 追加到环境变量echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrcecho '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm' >> ~/.zshrc# 重载环境变量source ~/.zshrc# 使用nvm安装18版本nodejsnvm install 18# 使用nodejs的npm安装pm2npm install -g pm2<br>```|

[![安装好nvm](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installNvm.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installNvm.webp "安装好nvm")

安装好nvm

[![安装好pm2](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installPm2.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/installPm2.webp "安装好pm2")

安装好pm2

之后，使用pm2的命令，启动hbbr和hbbs：

|   |   |
|---|---|
|1  <br>2  <br>3|```<br># 当前正在RustDesk的目录内pm2 start hbbspm2 start hbbr<br>```|

之后，就可以发现。RustDesk已经启动。 具体的pm2使用，以后有机会和大家讲解。

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%BD%BF%E7%94%A8Systemctl%E8%BF%90%E8%A1%8C)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E4%BD%BF%E7%94%A8Systemctl%E8%BF%90%E8%A1%8C "使用Systemctl运行")使用Systemctl运行

我其实挺喜欢用systemctl的；但是上篇文章，其实已经足够详细，这里就直接给配置的脚本吧，我们需要创建两个Server，首先是**hbbs**:

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8  <br>9  <br>10  <br>11  <br>12  <br>13|```<br>[Unit]Description=RustDesk HbbsAfter=network.target[Service]User=mintimateType=simpleWorkingDirectory=/home/mintimate/myApplication/RustDeskExecStart=/home/mintimate/myApplication/RustDesk/hbbsExecStop=/bin/kill -TERM $MAINPID[Install]WantedBy=multi-user.target<br>```|

之后是**hbbr**:

|   |   |
|---|---|
|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8  <br>9  <br>10  <br>11  <br>12  <br>13|```<br>[Unit]Description=RustDesk HbbrAfter=network.target[Service]User=mintimateType=simpleWorkingDirectory=/home/mintimate/myApplication/RustDeskExecStart=/home/mintimate/myApplication/RustDesk/hbbrExecStop=/bin/kill -TERM $MAINPID[Install]WantedBy=multi-user.target<br>```|

[![启动完成后](https://imagehost.mintimate.cn/post_guideToHostRustDesk/startRustDeskSystemctl.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/startRustDeskSystemctl.webp "启动完成后")

启动完成后

到此，使用Systemctl注册为服务就完成啦。其实也可以使用screen，就是重启系统后，再次启动比较麻烦。

# [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#Q-amp-A)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#Q-amp-A "Q&A")Q&A

个人的一些使用经验，还有一些常见的问题。

## [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E5%A6%82%E4%BD%95%E5%BC%80%E5%90%AF%E5%BC%BA%E5%88%B6%E5%8A%A0%E5%AF%86)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#%E5%A6%82%E4%BD%95%E5%BC%80%E5%90%AF%E5%BC%BA%E5%88%B6%E5%8A%A0%E5%AF%86 "如何开启强制加密")如何开启强制加密

如果你搭建好了RustDesk后，会发发现： 不需要设置key，也可以进行RustDesk的连接。

这是因为RustDesk默认服务器端会开启加密，但是并不强制。

如何你想开启强制加密，可以在启动hbbr和hbbs的时候，加上参数`-k _`:

|   |   |
|---|---|
|1  <br>2|```<br>./hbbr -k _./hbbs -k _<br>```|

这样，客户端建立的连接，就需要强制设置好key，也就是我们的`*.pub`文件，否则无法建立通讯和远程连接：  
[![强制密钥验证](https://imagehost.mintimate.cn/post_guideToHostRustDesk/forceKey.webp)](https://imagehost.mintimate.cn/post_guideToHostRustDesk/forceKey.webp "强制密钥验证")

强制密钥验证

# [](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#END)[](https://www.mintimate.cn/2023/08/27/guideToHostRustDesk/#END "END")END

好啦，本次的RustDesk演示就到这里；总的来说，还是很简单的；当然，对于小白用户可能会有一定的难度，建议配合教程和官方文档的同时，还需要配合一些网上Linux教程使用，尤其是亚马逊服务器的使用教程。

我在注册亚马逊服务器的时候，其实就卡了挺久的。

有什么问题，可以评论区留言哦。爱发电用户可以优先快速处理，我们有机会再见👋。