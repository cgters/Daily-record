# 自建 Shadowsocks 端服务器

Shadowsocks 方式汇总

1. 懒人必备：购买第三方服务商提供的套餐，当然是付费的

2. 穷逼必备：也有极少部分提供免费服务，只不过为了控制服务质量，会在一定时间内更新信息，效率难以保证

   提供一个：[ishadowx.com](http://ss.ishadowx.com/)

3. 不折腾会死星人：自建服务器



### 具体步骤

#### Shadowsocks 服务器端 配置操作步骤 

1. SSH 连接服务器

   > 一般各大服务商都有教程，操作方法也有所区别，查阅帮助文档吧

2. 安装 Shadowsocks 

   > 以下命令均在最高权限用户 root 下运行 
   >
   > 由于我配置的是 Ubuntu 系统，所以目前只有 Ubuntu 系统的
   >
   > ​
   >
   > Linux CentOS 的暂时没去尝试，网上有很多教程
   >
   > Linux 版本的 SS 分为 服务端 和 客户端 两个功能，前者是对外提供服务，后者是接受服务
   >
   > ​
   >
   > 为了方便，附带一份：
   >
   > https://loyalsoldier.me/deploy-ghost-on-centos-7/

   Sudo su // 获取 root 权限

   agt-get update // 更新软件源列表

   apt-get install python-pip // 安装 Python Pip 安装 Package 包服务框架

   pip instatll shadowsocks // 安装 Shadowsocks 组件

3. 配置 Shadowsocks 配置文件

   1. vi /etc/shadowsocks.json // 新建一个 config.json 

   2. 按 i 进入终端编辑模式，按照下面配置格式，修改相关信息

      > {
      >
      > "server": "my_server_ip",
      >
      > // 服务端监听地址( IPv4 或 IPv6 )
      > "server_port": 8388,
      >
      > // 为了安全，可修改为大于 1024 的数字
      > "local_address": "127.0.0.1",
      >
      >  // 本地监听地址，缺省为127.0.0.1
      >
      > "local_port": 1080,
      >
      > // 为了安全，可修改为大于 1024 的数字
      > "password": "mypassword",
      >
      > // 设置一个密码
      > "timeout": 300,
      >
      > // 超时时间（秒）
      > "method": "aes-256-cfb",
      >
      > // 加密方法
      > "fast_open": false
      >
      > // 是否启用[TCP-Fast-Open](https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open)，true或者false
      >
      > }

      按  ESC 退出编辑，按 :wq 保存并退出

4. 启动并永久运行 （ 开机自启 ）

   启动：ssserver -c /etc/shadowsocks.json -d start

   停止：ssserver -c /etc/shadowsocks.json -d stop

   重启：ssserver -c /etc/shadowsocks.json -d restart



​	设置开机自启/永久运行

​	vi /etc/rc.local

​	加入：

​	sudo ssserver -c /etc/shadowsocks/ss.json -d start

#### 客户端 配置

下载 Shadowsocks 客户端，由于各种问题，只能告诉去 GitHub 上搜搜，或者你可以直接用搜索引擎碰碰运气，去看看那些散落在各大软件门户网站的资源



安装成功后，右下角系统任务栏中，有一个纸飞机的图标

右击，点开 服务器 > 编辑服务器，即弹出配置界面

对应 json 配置文件，对照相同项目填写即可

配置完成后，点击勾选即可

> 建议，将允许来自局域网的链接



### PAC 功能（ 代理自动设置 ）

可以保证只为国外网站走代理，国内网站伊朗直连，从而实现加速国外网站的同时，保持国内网站的链接速度



Shadowsocks 的 PAC 功能是通过 Shadowsocks 软件目录下的 pac.txt（ 默认自动翻墙的网站列表）和 user-rule.txt （ 用户自己设置的需要翻墙的网站列表 ）两个文件同时实现的。



user-rule.txt 文件中，网址前面有 @@ 表示该网站不需要翻墙

每次编辑完后，需要选择从 GFWList 更新本地 PAC 才能立即生效 