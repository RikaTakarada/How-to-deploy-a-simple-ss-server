# 如何部署属于你自己的 SS server

这个教程面向所有不懂计算机的人，希望看完这篇教程以后，都能学会如何搭建 SS server 来科学上网。搭完以后是可以给你身边的人一起使用的。

## 目录
* [1. 购买属于你自己的云服务器](#1)
* [1.1 科学上网简单原理](#1.1)
* [1.2 选择云服务器供应商](#1.2)
* [1.3 如何选择云服务器规格](#1.3)
* [1.4 购买流程](#1.4)
* [2. 在云服务器上部署 SS](#2)
* [2.5 下载 shadowsocks](#2.1)
* [2.4 使用 ss-bash 辅助工具](#2.2)
* [2.3 防火墙](#2.3)
* [3. 在自己电脑、手机上使用客户端连接 SS server](#3)
* [3.1 Windows](#3.1)
* [3.2 Mac](#3.2)
* [3.3 Android](#3.3)
* [3.4 iOS](#3.4)

<h2 id="1">1. 购买属于你自己的云服务器</h2>

<h3 id="1.1">1.1 科学上网简单原理</h3>

简单谈谈科学上网是如何实现的。

你的计算机 A 在国内受到 GFW 限制，因此 A 无法访问某些网站。但是如果你有一台境外的服务器 B，那么我们可以把 A 的所有的网络请求都发给 B,设备 B 在 GFW 之外处理好这些网络请求之后再发给 A，由此实现科学上网。

所以首先我们需要购买一台境外的服务器。

<h3 id="1.2">1.2 选择云服务器供应商</h3>

列举几个我知道的境外服务器供应商。境外的一些支付方式可能并不是所有人都方便，因此推荐几个能够使用支付宝(Alipay)支付的供应商。

如果有VISA、PAYPAL的话，其他运营商也可以考虑，比如亚马逊 AWS、Digital Ocean 等。

| 供应商 | 可支付方式 | 最低价格 |
|--------|------------|----------|
| Vultr  | 支付宝(Alipay)，微信(WeChat) | $3.5/mo ≈ 24.32 RMB/月 |
| Conoha | 支付宝(Alipay) | 900円/mo ≈ 55.39 RMB/月 |
| Bandwagon | 听说现在支持支付宝(Alipay)了，不过我没有用过 | $19.99/yr ≈ 11.58 RMB/月 |

接下来谈一下我用过的运营商的优点和缺点。读者可以根据自己需求选择，或者选择其他运营商。

#### Vultr
- 优点
    - 主页没有被墙
        - 有些境外云服务器运营商被墙了，于是就陷入一个死循环。想要科学上网，就得先购买境外云服务器，想要购买境外云服务器，就得先科学上网。十分尴尬。
    - 除了支付宝(Alipay)还支持微信(Wechat)支付
    - 服务器地点选择很多
        - 东京，新加坡，伦敦等等
- 缺点
    - IP 段有问题
        - 使用 Google 的时候，用 Vultr 的服务器上网的话，很大概率会把你识别为机器人，经常需要输入验证码，很麻烦。而且因为这个问题，导致<b>无法使用 Google Scholar</b>
        - 识别为机器人还算好的。这里有些服务器的 IP 甚至都被墙了，导致你根本无法连接你的服务器，根本没法科学上网。这个时候你需要换个地方的服务器，当然这是后话了。
        - 似乎也是因为 IP 段的问题，无法访问 Pixiv

#### Conoha
- 优点
    - 稳定的日本云服务器
        - 可以玩舰○、F○O 等游戏。
    - 支付宝支付
- 缺点
    - 贵
        - 跟其他运营商比起来 Conoha 的主机真的很贵，还有 8% 消费税。2019年消费税还要涨……
    - 服务器地点选择较少
        - 只有新加坡和东京
    - 主页被墙
        - 那你得先问别人借个科学上网…

#### Bandwagon
- 优点
    - 便宜
    - 听说有香港的服务器
- 缺点
    - IP段有问题
        - 听说比 Vultr 服务器挂掉的概率还大。意思就是，可能你买了一年份的 Bandwagon，结果才用几个月，服务器就被墙了，访问不了了……
    - 主页被墙

#### 总结

有日本 IP 需求的请直接购买 Conoha；有学术需求需要使用 Google Scholar 的也请购买 Conoha；只是想刷刷 twi、ins、看看 youtube 的话，在 Bandwagon、Vultr 间取舍吧。

个人购买了 Vultr 和 Conoha 两家的服务器，防止某一家的服务器挂了，我还有一台服务器能上网。

<h3 id="1.3">1.3 如何选择云服务器规格</h3>

由于我们只是用来上网，其实购买最低配就够了。

- 注意：Vultr 最低配只支持 IPv6

<h3 id="1.4">1.4 购买流程 (之后补图)</h3>

我们以 Vultr 为例子。省略注册流程。基本都是向账户里充钱，月初扣费的机制。

注册完之后进入 https://my.vultr.com 的控制台界面。印象里你得先充钱才能部署云服务器。点击左侧菜单中的 Billing 

![billing](./whatever)

在 Make Payment 选项卡下，选择 Alipay(支付宝) 支付方式，然后先充点钱。

![alipay](./whatever)

每个页面的右上角都有一个蓝色的加号，点击该加号进入选择云服务器的界面

![deploy](./whatever)

就用默认的 Vultr Cloud Compute (VC2) 即可。首先看 Server Location 选择服务器的地点。

地点有很多，推荐新加坡(Singapore)，用了很久都没有问题。其他的地方的服务器都很有可能被墙。

![location](./whatever)

然后看 Server Type 服务器类型。这里以 64 bit OS -> CentOS 7 为例。也可以根据个人喜好，选择 Ubuntu 等其他 Linux 服务器。(别选 Windows 就好)

![type](./whatever)

最后选择 Server Size，$3.5/mo 就可以了，500 GB 带宽。

也可以根据自己需要购买 $5/mo 的，有 1000 GB 带宽。

![size](./whatever)

后面的选项都可以跳过无视。点击右下角的 Deploy Now，然后在 Servers 界面可以看到刚刚选购好的云服务器，等待几分钟就启动完毕。

![servers](./whatever)

- <b>注意</b>
    - 选好之后，先检查服务器是否被墙
    - Servers 界面能够看到我们的云服务器的 IP 地址
        - 假设我们的 IP 地址为 44.55.66.111
    - 打开命令行，输入以下命令(`$` 符号不要输入)
    ```
    $ ping 44.55.66.111
    ```
    - 如果看到不断 Time Out 连接超时，则意味着服务器被墙了
    - 那么需要在 Servers 界面销毁该服务器，回到开始的步骤，重新选择服务器
        ![destory](./whatever)
    - 按时间收费，因此选择服务器后，销毁，再选择，这个过程几乎不会扣费

<h2 id="2">2. 在云服务器上部署 SS</h2>

<h3 id="2.1">2.1 连接服务器</h3>

通过 SSH 进行连接，windows 用户可以先下载一个 cmder / termius / putty 等工具。Mac 用户则不需要下载任何辅助工具，Mac 自带 SSH。

这里以 cmder 为例子。![官网下载地址](http://cmder.net/) 选择 Download Mini 即可。

在 Servers 界面可以看到我们刚刚部署好的服务器，点击查看详情，可以看到 Username(用户名):root, Password(密码): 一串乱码,IP Address(IP地址): xx.xx.xxx.xxx。

假设我们的 IP 地址为 44.55.66.111

Windows 用户打开 cmder，Mac 用户打开终端(Terminal)，输入以下命令

```
$ ssh root@44.55.66.111
```

会出现提示让你输入密码

```
$ ssh root@44.55.66.111
root@44.55.66.111's password:
```
输入刚刚 Servers 界面看到的密码。

如果出现一串英文提示，问你是否信任该 SSH 连接，输入 yes 信任即可。

然后我们就连上了我们的云服务器。

<h3 id="2.2">2.2 下载 Shadowsocks</h3>

之前提到过，以 CentOS 为例。

输入以下命令：

```
$ yum install python-setuptools
$ easy_install pip
$ pip install shaodowsocks
```

主要做了三件事：下载python，下载pip，下载shaodowsocks

<h3 id="2.3">2.3 使用 ss-bash 辅助工具</h3>

使用了来自 ![hellofwy/ss-bash](https://github.com/hellofwy/ss-bash) 的管理脚本。帮助我们配置 SS 以及监控流量使用情况。

输入以下命令

```
$ yum install git
$ git clone https://github.com/hellofwy/ss-bash.git
```

然后输入命令进入到 ss-bash 文件夹进行相关操作
```
$ cd ss-bash
```

新建用户。比如新建用户端口 8388，密码为 passwd，流量限额 10G。（由于sudo命令需要获得权限，输完这行指令后，需要你输一下该服务器 root 用户的密码）
```
$ sudo ss-bash/ssadmin.sh add 8388 passwd 10G
```

启动 SS Server
```
$ sudo ss-bash/ssadmin.sh start
```

该脚本的其他功能可以查看 ![ss-bash中文说明](https://github.com/hellofwy/ss-bash/wiki)

- 注意:如果提示缺少某个 package，比如缺少 bc，则执行命令 `yum install bc` 即可解决

到这一步通常来说 SS server 已经配置完成，可以先到下一部分尝试连接服务器，看看能否科学上网。如果不行，再回头看后面关于防火墙的部分

<h3 id="2.4">2.4 防火墙</h3>

有些云服务器默认是开启防火墙的，也就是说，防火墙会阻止你的手机或计算机通过刚刚设置的端口连接你的云服务器。每个系统的防火墙设置也是不同的。这里仍然以 CentOS 7 为例讲解如何开启防火墙端口。

比如我们刚刚新建的用户端口为 8388，则需要执行以下命令。首先开放8388端口，然后重启防火墙。

```
$ firewall-cmd --zone=public --add-port=8388/tcp --permanent
$ firewall-cmd --reload
```

可以通过以下命令查看已开放的端口
```
$ firewall-cmd --list-ports
```
<h2 id="3"> 在自己电脑、手机上使用客户端连接 SS server</h2>

<h3 id="3.1">3.1 Windows</h3>

![ssr客户端下载](https://github.com/shadowsocksr-backup/shadowsocksr-csharp/releases)

在这个网址中下载最新版 ShadowsocksR。不要下载 Source Code。

下载好之后解压，打开红色小飞机，编辑服务器列表。以我们之前的过程为例。
- 服务器(Server): 44.55.66.111 
- 端口(Port)：8388
- 密码(Password): passwd

其余不需要做任何变动。

然后开启 SSR，选择 PAC 模式/全局模式 即可科学上网。

<h3 id="3.2">3.2 Mac</h3>

![ss客户端下载](https://github.com/shadowsocks/shadowsocks-iOS/releases)


基本过程同理 Windows

<h3 id="3.3">3.3 Android</h3>

![ss客户端下载](https://github.com/shadowsocks/shadowsocks-android/releases)

建议下载 latest release 而不是 pre-release

选择 univiersal 下载即可。

之后过程同理

<h3 id="3.4">3.4 iOS</h3>

如果你有外区 App Store 账号，直接在外区购买一个 Shadowrocket。

如果没有，则需要在电脑上安装PP手机助手，手机连接电脑，用PP手机助手安装 Shadowrocket

之后过程同理
