title: 中间人攻击
speaker: Brickgao
url: https://github.com/ksky521/nodePPT
transition: cards
files: /js/demo.js,/css/demo.css

[slide]

# 中间人攻击
## Man-in-the-Middle Attack

[slide]

## 什么是中间人攻击？
--------------------

{:.flexbox.vcenter}

![](/img/hack.png)

### 简单点来说中间人攻击就是网络你和网络之间的第三人，攻击者充当了 client 和 server 之间的转发者，作为转发者他可以修改通信中的内容，而 client 和 server 不会知道中间有转发者，双方均认为是与对方直接通信。

[slide]

## 我们举一个加密通信中的栗子

*   alice == "嗨，Bob，我是Alice。给我你的公钥" ==> Mallory                  Bob

*   alice                  Mallory == "嗨，Bob，我是Alice。给我你的公钥" ==> Bob

*   alice                                       Mallory <== [ Bob 的公钥] == Bob

*   alice <== [ Mallory 的公钥] == Mallory                                   Bob

*   alice == "我们在公共汽车站见面！" [使用 Mallory 的公钥加密] ==> Mallory  Bob

*   alice                  Mallory == "在家等我！" [使用 Bob 的公钥加密] ==> Bob

这种情况下， Bob 认为信息是直接从 Alice 哪里传过来的。

[slide]
## 如何进行中间人攻击？
----------------------

我们以 Ettercap 为例子进行攻击

Ettercap 是一个局域网中间人攻击的工具

我们可以在 https://github.com/Ettercap/ettercap/ 得到它的源码，编译后安装

也可以直接获取它的软件包，例如在 ubuntu 下输入

```bash
     sudo apt-get install ettercap-graphical
```

[slide]

{:.flexbox.vcenter}

![](/img/mitm.png)
    
[slide]
## dns 欺诈攻击
-----------------------

我们先修改 etter.dns 中的规则：

```bash
     *.com   A   202.114.64.60
```

修改完成后在图形界面中载入插件，选择目标，开启 arp 攻击，然后进行欺诈。

也可以直接在终端里完成：

```bash
     root@pc:~# ettercap -T -q -i eth0 -P dns_spoof // //
```

这样的话，我们只要访问`*.com`的站点都会重定向至 http://my.whu.edu.cn

    
[slide]
## dns 欺诈攻击
-----------------------

那么，这项技术能够用来做什么？

例如：Block 掉一些网站

TIP：域名服务器缓存投毒（DNS cache poisoning），又名域名服务器缓存污染（DNS cache pollution），是指一些刻意制造或无意中制造出来的域名服务器数据包，把域名指往不正确的IP地址。

[slide]
## dns 欺诈攻击
----------

比如我们在国内 ping `twitter.com`：

```bash
     $ ping twitter.com

     正在 Ping twitter.com [59.24.3.173] 具有 32 字节的数据:
     请求超时。

     59.24.3.173 的 Ping 统计信息:
         数据包: 已发送 = 1，已接收 = 0，丢失 = 1 (100% 丢失)，
```

[slide]
## dns 欺诈攻击
----------

`twitter.com`被指向到`59.24.3.173`，然后？

然后你就上不去了...

查了一下 wiki 刚好是某长城会将黑名单内的域名污染到的IP地址。

[slide]
## dns 欺诈攻击
----------

再例如：钓鱼

以一个武大无线网登录的钓鱼为例。

我们写一个钓鱼网站，然后将`*.*`指向这个网站。

[slide]
## dns 欺诈攻击
-----------------------

再再例如：运营商用 dns 欺诈来插入广告

[slide]
##对 ssl 的攻击
----------

如果不是加密初始化的时候攻击，对 ssl 的攻击是不大行的通的，主要是因为如果你对 ssl 通信进行攻击，那么因为 PKI 的因素，我们会在浏览器上看到这个：

{:.flexbox.vcenter}

![](/img/github-attack.png)

[slide]
##对 ssl 的攻击
----------

这里我们提供一个思路，通过`sslstrip`将所有的 https 连接重置为 http 连接，这样我们可以通过监听广播来过滤用户名和密码。

你可以在 https://github.com/moxie0/sslstrip 上获取源码，或者直接获取软件包，例如在 ubuntu 获取`sslstrip`：

```bash
      sudo apt-get install sslstrip
```

具体可以参考 http://www.freebuf.com/articles/web/5929.html

[slide]
##获取 cookie 
-----------

开启中间人攻击选项，监听连接，寻找目标网站，并截取 cookie，最后用 Tamper Data 或其他工具修改 cookie，就可以实现成功的登录。

{:.flexbox.vcenter}

![](/img/get_cookie.png)

[slide]
## 如何防范中间人攻击
-----------

对于使用 ARP 攻击来实现中间人攻击的方法，我们可以绑定网关。

[slide]
## 如何防范中间人攻击
-----------

建立信任体系，使用证书、数字签名。

[slide]
## 如何防范中间人攻击
----------

当然，

手里掌握权力的人，只有道德能够防止他做坏事。

[slide]
## 如何防范中间人攻击
----------

这也是为什么我不信任 CNNIC 证书以及自签名证书。
