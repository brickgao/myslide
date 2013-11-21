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

也可以直接在 bash 里完成：

```bash
     root@pc:~# ettercap -T -q -i eth0 -P dns_spoof // //
```

这样的话，我们只要访问`*.com`的站点都会重定向至 http://my.whu.edu.cn

[slide]
##对 ssl 的攻击
----------

基本上对 ssl 的攻击是不大行的通的，主要是因为如果你对 ssl 通信进行攻击，那么因为 PKI 的因素，我们会在浏览器上看到这个：

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


