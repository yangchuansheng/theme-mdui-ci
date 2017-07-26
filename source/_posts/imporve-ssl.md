---
title: 失踪人口回归系列之增强站点SSL安全性
date: 2017/03/22 19:39:00
categories:
- 技术
tags:
- 教程
---
失踪人口回归系列之第二篇
<!--more-->

经过这篇文章后，我的SSL Lab的测试也是A+了。

- 原文地址：[https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html](https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html)
- 原文作者：[Remy van Elst](https://raymii.org/)
- 译者与校对：[wxy](https://github.com/wxy)
- 本文由[LCTT](https://github.com/LCTT/TranslateProject)原创翻译，[Linux中国](http://linux.cn/) 荣誉推出
- 此为原文重排版

本文向你介绍如何在 nginx 服务器上设置健壮的 SSL 安全机制。我们通过禁用 SSL 压缩来降低 CRIME 攻击威胁；禁用协议上存在安全缺陷的 SSLv3 及更低版本，并设置更健壮的加密套件（cipher suite）来尽可能启用前向安全性（Forward Secrecy）；此外，我们还启用了 HSTS 和 HPKP。这样我们就拥有了一个健壮而可经受考验的 SSL 配置，并可以在 Qually Labs 的 SSL 测试中得到 A 级评分。

本教程可以通过[发布于 2014/1/21 的 SSL 实验室测试的严格要求](http://blog.ivanristic.com/2014/01/ssl-labs-stricter-security-requirements-for-2014.html)（我之前就通过了测试，如果你按照本文操作就可以得到一个 A+ 评分）。

- 本教程也可用于 Apache
- 本教程也可用于 Lighttpd
- 本教程也可用于 FreeBSD, NetBSD 和 OpenBSD 上的 nginx ，放在[BSD Now 播客上](http://www.bsdnow.tv/tutorials/nginx)
你可以从下列链接中找到这方面的进一步内容：

- [野兽攻击（BEAST）](https://en.wikipedia.org/wiki/Transport_Layer_Security#BEAST_attack)
- [罪恶攻击（CRIME）](https://en.wikipedia.org/wiki/CRIME_%28security_exploit%29)
- [怪物攻击（FREAK ）](http://blog.cryptographyengineering.com/2015/03/attack-of-week-freak-or-factoring-nsa.html)
- [心血漏洞（Heartbleed）](http://heartbleed.com/)
- [完备的前向安全性（Perfect Forward Secrecy）](https://en.wikipedia.org/wiki/Perfect_forward_secrecy)
- [RC4 和 BEAST 的处理](https://en.wikipedia.org/wiki/Transport_Layer_Security#Dealing_with_RC4_and_BEAST)

**<big>在编辑之前切记备份一下配置文件！</big>**

### 我是懒人
那你直接看我从[Cipherli.st](https://cipherli.st/)上得到的内容复制粘贴到你的站点配置就好了。
#### Apache
````
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
SSLProtocol All -SSLv2 -SSLv3
SSLHonorCipherOrder On
# HSTS
Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
Header always set X-Frame-Options DENY
Header always set X-Content-Type-Options nosniff
# Apache版本 >= 2.4
SSLCompression off
SSLUseStapling on
SSLStaplingCache "shmcb:logs/stapling-cache(150000)"
# Apache版本 >= 2.4.11
SSLSessionTickets Off
````
#### nginx
````
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_ecdh_curve secp384r1; # nginx版本 >= 1.1.0
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off; # nginx版本 >= 1.5.9
ssl_stapling on; # Requires nginx >= 1.3.7
ssl_stapling_verify on; # nginx版本 => 1.3.7
resolver $DNS-IP-1 $DNS-IP-2 valid=300s; # 此处将$DNS-IP-1和$DNS-IP-2更换为DNS服务商的IP
resolver_timeout 5s;
add_header Strict-Transport-Security "max-age=63072000; #HSTS includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
````
#### Lighttpd
````
ssl.honor-cipher-order = "enable"
ssl.cipher-list = "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH"
ssl.use-compression = "disable"
setenv.add-response-header = (
    "Strict-Transport-Security" => "max-age=63072000; includeSubDomains; preload",
    "X-Frame-Options" => "DENY",
    "X-Content-Type-Options" => "nosniff"
)
ssl.use-sslv2 = "disable"
ssl.use-sslv3 = "disable"
````

### 我不是懒人
#### 方便快捷修改站点配置文件
##### SSLv2 和 SSLv3
SSLv2 是不安全的，所以我们需要禁用它。我们也禁用 SSLv3，因为 TLS 1.0 在遭受到降级攻击时，会允许攻击者强制连接使用 SSLv3，从而禁用了前向安全性（forward secrecy）。

编辑站点配置文件：
````
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
````

##### 加密套件（cipher suite）
前向安全性（Forward Secrecy）用于在长期密钥被破解时确保会话密钥的完整性。PFS（完备的前向安全性）是指强制在每个/每次会话中推导新的密钥。

这就是说，泄露的私钥并不能用来解密（之前）记录下来的 SSL 通讯。

提供PFS（完备的前向安全性）功能的是那些使用了一种 Diffie-Hellman 密钥交换的短暂形式的加密套件。它们的缺点是系统开销较大，不过可以使用椭圆曲线的变体来改进。

以下两个加密套件是我推荐的，之后[Mozilla 基金会](https://wiki.mozilla.org/Security/Server_Side_TLS)也推荐了。

推荐的加密套件：
````
ssl_ciphers 'AES128+EECDH:AES128+EDH';
````
向后兼容的推荐的加密套件（IE6/WinXP）：
````
ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
````
如果你的 OpenSSL 版本比较旧，不可用的加密算法会自动丢弃。应该一直使用上述的完整套件，让 OpenSSL 选择一个它所支持的。

加密套件的顺序是非常重要的，因为其决定了优先选择哪个算法。上述优先推荐的算法中提供了PFS（完备的前向安全性）。

较旧版本的 OpenSSL 也许不能支持这个算法的完整列表，AES-GCM 和一些 ECDHE 算法是相当新的，在 Ubuntu 和 RHEL 中所带的绝大多数 OpenSSL 版本中不支持。

##### 更多设置
确保你也添加了如下行：
````
ssl_prefer_server_ciphers on;
ssl_session_cache shared:SSL:10m;
````
在一个 SSLv3 或 TLSv1 握手过程中选择一个加密算法时，一般使用客户端的首选算法。如果设置了上述配置，则会替代地使用服务器端的首选算法。

- [关于 ssl_prefer_server_ciphers 的更多信息](http://wiki.nginx.org/HttpSslModule#ssl_prefer_server_ciphers)
- [关于 ssl_ciphers 的更多信息](http://wiki.nginx.org/HttpSslModule#ssl_ciphers)

##### 前向安全性和 Diffie Hellman Ephemeral （DHE）参数
前向安全性（Forward Secrecy）的概念很简单：客户端和服务器协商一个永不重用的密钥，并在会话结束时销毁它。服务器上的 RSA 私钥用于客户端和服务器之间的 Diffie-Hellman 密钥交换签名。从 Diffie-Hellman 握手中获取的预主密钥会用于之后的编码。因为预主密钥是特定于客户端和服务器之间建立的某个连接，并且只用在一个限定的时间内，所以称作短暂模式（Ephemeral）。

使用了前向安全性，如果一个攻击者取得了一个服务器的私钥，他是不能解码之前的通讯信息的。这个私钥仅用于 Diffie Hellman 握手签名，并不会泄露预主密钥。Diffie Hellman 算法会确保预主密钥绝不会离开客户端和服务器，而且不能被中间人攻击所拦截。

所有版本的 nginx（如1.4.4）都依赖于 OpenSSL 给 Diffie-Hellman （DH）的输入参数。不幸的是，这意味着 Diffie-Hellman Ephemeral（DHE）将使用 OpenSSL 的默认设置，包括一个用于密钥交换的1024位密钥。因为我们正在使用2048位证书，DHE 客户端就会使用一个要比非 DHE 客户端更弱的密钥交换。

我们需要生成一个更强壮的 DHE 参数：
```` bash
$ cd /etc/ssl/certs
$ openssl dhparam -out dhparam.pem 4096
````
然后告诉 nginx 将其用作 DHE 密钥交换：
````
ssl_dhparam /etc/ssl/certs/dhparam.pem;
````
##### OCSP 装订（Stapling）
当连接到一个服务器时，客户端应该使用证书吊销列表（CRL）或在线证书状态协议（OCSP）记录来校验服务器证书的有效性。CRL 的问题是它已经增长的太大了，永远也下载不完了。

OCSP 更轻量级一些，因为我们每次只请求一条记录。但是副作用是当连接到一个服务器时必须对第三方 OCSP 响应器发起 OCSP 请求，这就增加了延迟和带来了潜在隐患。事实上，CA 所运营的 OCSP 响应器非常不可靠，浏览器如果不能及时收到答复，就会静默失败。攻击者通过 DoS 攻击一个 OCSP 响应器可以禁用其校验功能，这样就降低了安全性。

解决方法是允许服务器在 TLS 握手中发送缓存的 OCSP 记录，以绕开 OCSP 响应器。这个机制节省了客户端和 OCSP 响应器之间的通讯，称作 OCSP 装订。

客户端会在它的 CLIENT HELLO 中告知其支持 status_request TLS 扩展，服务器仅在客户端请求它的时候才发送缓存的 OCSP 响应。

大多数服务器最多会缓存 OCSP 响应48小时。服务器会按照常规的间隔连接到 CA 的 OCSP 响应器来获取刷新的 OCSP 记录。OCSP 响应器的位置可以从签名的证书中的授权信息访问（Authority Information Access）字段中获得。

- [阅读我的教程：在 NGINX 中启用 OCSP 装订](https://raymii.org/s/tutorials/OCSP_Stapling_on_nginx.html)

##### HTTP 严格传输安全（HSTS）
如有可能，你应该启用[HTTP 严格传输安全（HSTS）](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)，它会引导浏览器和你的站点之间的通讯仅通过 HTTPS。
- [阅读我关于 HSTS 的文章，了解如何配置它](https://linux.cn/article-5266-1.html)

##### HTTP 公钥固定扩展（HPKP）
你也应该启用[HTTP 公钥固定扩展（HPKP）](https://wiki.mozilla.org/SecurityEngineering/Public_Key_Pinning)。

公钥固定的意思是一个证书链必须包括一个白名单中的公钥。它确保仅有白名单中的 CA 才能够为某个域名签署证书，而不是你的浏览器中存储的任何 CA。

我已经写了一篇[]关于 HPKP 的背景理论及在 Apache、Lighttpd 和 NGINX 中配置例子的文章](https://linux.cn/article-5282-1.html)。

#### 调整服务器
##### 野兽攻击（BEAST）和 RC4
简单的说，野兽攻击（BEAST）就是通过篡改一个加密算法的 CBC（密码块链）的模式，从而可以对部分编码流量悄悄解码。更多信息参照上面的链接。

针对野兽攻击（BEAST），较新的浏览器已经启用了客户端缓解方案。推荐方案是禁用 TLS 1.0 的所有加密算法，仅允许 RC4 算法。然而，[]针对 RC4 算法的攻击也越来越多](http://www.isg.rhul.ac.uk/tls/) ，很多已经从理论上逐步发展为实际可行的攻击方式。此外，有理由相信 NSA 已经实现了他们所谓的“大突破”——攻破 RC4 。

禁用 RC4 会有几个后果。其一，当用户使用老旧的浏览器时，比如 Windows XP 上的 IE 会用 3DES 来替代 RC4。3DES 要比 RC4 更安全，但是它的计算成本更高，你的服务器就需要为这些用户付出更多的处理成本。其二，RC4 算法能减轻 野兽攻击（BEAST）的危害，如果禁用 RC4 会导致 TLS 1.0 用户会换到更容易受攻击的 AES-CBC 算法上（通常服务器端的对野兽攻击（BEAST）的“修复方法”是让 RC4 优先于其它算法）。我认为 RC4 的风险要高于野兽攻击（BEAST）的风险。事实上，有了客户端缓解方案（Chrome 和 Firefox 提供了缓解方案），野兽攻击（BEAST）就不是什么大问题了。而 RC4 的风险却在增长：随着时间推移，对加密算法的破解会越来越多。

##### 怪物攻击（FREAK）
怪物攻击（FREAK）是一种中间人攻击，它是由来自[INRIA、微软研究院和 IMDEA](https://www.smacktls.com/)的密码学家们所发现的。怪物攻击（FREAK）的缩写来自“Factoring RSA-EXPORT Keys（RSA 出口密钥因子分解）”

这个漏洞可上溯到上世纪九十年代，当时美国政府禁止出口加密软件，除非其使用编码密钥长度不超过512位的出口加密套件。

这造成了一些现在的 TLS 客户端存在一个缺陷，这些客户端包括： 苹果的 SecureTransport 、OpenSSL。这个缺陷会导致它们会接受出口降级 RSA 密钥，即便客户端并没有要求使用出口降级 RSA 密钥。这个缺陷带来的影响很讨厌：在客户端存在缺陷，且服务器支持出口降级 RSA 密钥时，会发生中间人攻击，从而导致连接的强度降低。

攻击分为两个组成部分：首先是服务器必须接受“出口降级 RSA 密钥”。

中间人攻击可以按如下流程：

在客户端的 Hello 消息中，要求标准的 RSA 加密套件。
中间人攻击者修改该消息为‘export RSA’（输出级 RSA 密钥）。
服务器回应一个512位的输出级 RSA 密钥，并以其长期密钥签名。
由于 OpenSSL/SecureTransport 的缺陷，客户端会接受这个弱密钥。
攻击者根据 RSA 模数分解因子来恢复相应的 RSA 解密密钥。
当客户端编码‘pre-master secret’（预主密码）给服务器时，攻击者现在就可以解码它并恢复 TLS 的‘master secret’（主密码）。
从这里开始，攻击者就能看到了传输的明文并注入任何东西了。
本文所提供的加密套件不启用输出降级加密，请确认你的 OpenSSL 是最新的，也强烈建议你将客户端也升级到新的版本。

##### SSL 压缩（罪恶攻击 CRIME）
罪恶攻击（CRIME）使用 SSL 压缩来完成它的魔法，SSL 压缩在下述版本是默认关闭的： nginx 1.1.6及更高/1.0.9及更高（如果使用了 OpenSSL 1.0.0及更高）， nginx 1.3.2及更高/1.2.2及更高（如果使用较旧版本的 OpenSSL）。

如果你使用一个早期版本的 nginx 或 OpenSSL，而且你的发行版没有向后移植该选项，那么你需要重新编译没有一个 ZLIB 支持的 OpenSSL。这会禁止 OpenSSL 使用 DEFLATE 压缩方式。如果你禁用了这个，你仍然可以使用常规的 HTML DEFLATE 压缩。

##### 卷毛狗攻击（POODLE）和 TLS-FALLBACK-SCSV
SSLv3 会受到[卷毛狗漏洞（POODLE）](https://raymii.org/s/articles/Check_servers_for_the_Poodle_bug.html)的攻击。这是禁用 SSLv3 的主要原因之一。

Google 提出了一个名为[TLSFALLBACKSCSV](https://tools.ietf.org/html/draft-ietf-tls-downgrade-scsv-00)的SSL/TLS 扩展，它用于防止强制 SSL 降级。如果你升级 到下述的 OpenSSL 版本会自动启用它。
- OpenSSL 1.0.1 带有 TLS_FALLBACK_SCSV 1.0.1j 及更高。
- OpenSSL 1.0.0 带有 TLS_FALLBACK_SCSV 1.0.0o 及更高。
- OpenSSL 0.9.8 带有 TLS_FALLBACK_SCSV 0.9.8zc 及更高。
[更多信息请参照 NGINX 文档](http://wiki.nginx.org/HttpSslModule#ssl_protocols)

#### 其他
##### 优先顺序的逻辑
- ECDHE+AESGCM 加密是首选的。它们是 TLS 1.2 加密算法，现在还没有广泛支持。当前还没有对它们的已知攻击。
- PFS 加密套件好一些，首选 ECDHE，然后是 DHE。
- AES 128 要好于 AES 256。有一个关于 AES256 带来的安全提升程度是否值回成本的讨论，结果是显而易见的。目前，AES128 要更值一些，因为它提供了不错的安全水准，确实很快，而且看起来对时序攻击更有抵抗力。
- 在向后兼容的加密套件里面，AES 要优于 3DES。在 TLS 1.1及其以上，减轻了针对 AES 的野兽攻击（BEAST）的威胁，而在 TLS 1.0上则难以实现该攻击。在非向后兼容的加密套件里面，不支持 3DES。
- RC4 整个不支持了。3DES 用于向后兼容。参看[#RC4_weaknesses](https://wiki.mozilla.org/Security/Server_Side_TLS#RC4_weaknesses)中的讨论。
##### 强制丢弃的算法
- aNULL 包含了非验证的 Diffie-Hellman 密钥交换，这会受到中间人（MITM）攻击
- eNULL 包含了无加密的算法（明文）
- EXPORT 是老旧的弱加密算法，是被美国法律标示为可出口的
- RC4 包含的加密算法使用了已弃用的 ARCFOUR 算法
- DES 包含的加密算法使用了弃用的数据加密标准（DES）
- SSLv2 包含了定义在旧版本 SSL 标准中的所有算法，现已弃用
- MD5 包含了使用已弃用的 MD5 作为哈希算法的所有算法

### 配置范例
````
server {

  listen [::]:443 default_server;

  ssl on;
  ssl_certificate_key /etc/ssl/cert/raymii_org.pem;
  ssl_certificate /etc/ssl/cert/ca-bundle.pem;

  ssl_ciphers 'AES128+EECDH:AES128+EDH:!aNULL';

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_session_cache shared:SSL:10m;

  ssl_stapling on;
  ssl_stapling_verify on;
  resolver 8.8.4.4 8.8.8.8 valid=300s;
  resolver_timeout 10s;

  ssl_prefer_server_ciphers on;
  ssl_dhparam /etc/ssl/certs/dhparam.pem;

  add_header Strict-Transport-Security max-age=63072000;
  add_header X-Frame-Options DENY;
  add_header X-Content-Type-Options nosniff;

  root /var/www/;
  index index.html index.htm;
  server_name raymii.org;

}
````
