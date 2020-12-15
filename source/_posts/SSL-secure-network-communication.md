---
title: 计算机下SSL安全网络通信
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-14 08:21:38
password:
summary: SSL介于应用层和网络层之间，位于传输层。应用的数据经过传输层中的SSL进行加密，并增加自己的SSL头，在将加密后的数据传向网络层。通过openssl可以自行签发证书，有了证书和私钥后就可以进行SSL安全通信了。
tags: 
- SSL
- 网络通信
- 安全
categories: 
- 计算机
---

# SSL安全套接层

**SSL安全套接层（Secure Socket Layer）** 为Netspace所研发，用以保证在Internet上数据传输的完整性和保密性，目前版本为3.0，最新版本为TLS1.2安全传输层协议（Transport Layer Security）。TLS和SSL两者差别极小可以将其理解为SSL的更新版本。


# 通信过程

**SSL介于应用层和网络层之间，位于传输层**。应用的数据经过传输层中的SSL进行加密，并增加自己的SSL头，在将加密后的数据传向网络层。

通过openssl可以自行签发证书，有了证书和私钥后就可以进行SSL安全通信了。

**调用SSL提供的接口流程如下图所示：**

1. 客户端的浏览器向服务器传送其所支持的SSL协议的版本号，加密算法，随机数，已经其他通讯所需的信息；

2. 服务端根据客户端传送的信息选择双方都支持的SSL协议版本，加密算法等信息和自己的证书传送到客户端（该步骤中可能会存在安全隐患，例如伪造客户端可支持的SSL协议版本过低，可能会导致服务端采取安全等级过低的SSL版本来进行通信）；

3. 客户端利用服务器传送过来的信息来验证服务器的合法性，包括证书是否已经过期、证书的颁发机构是否可靠、公钥是否匹配、证书上的域名和服务器端的是否匹配等，如果验证不通过则断开通讯，验证通过则继续通讯；

4. 客户端随机产生一个用于通讯加密的对称密码，通过公钥对其进行加密，然后将加密后的预主密码传给服务器；

5. 服务器通过自己的私钥进行解密来获得通讯加密所使用的预主密码，然后使用一系列算法来产生主通讯密码（客户端也将通过相同的方式来产生相同的主通讯密码）；

6. 客户端向服务器发出信息，指明后续的数据通讯将使用加解密的主密码为上以阶段生成的主通讯密码，同时通知服务端客户端的握手过程结束；

7. 服务端同样想客户端指明通讯密码并确认握手过程结束；

8. 握手过程结束。SSL安全通道数据通讯开始，客户端和服务器开始使用相同的对称秘钥进行数据通讯，同时进行通讯完整性校验。

![调用SSL提供的接口流程](https://img-blog.csdnimg.cn/20201214080900848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70) 

# SSL通讯建立后

从上述通讯过程可以看出，在ssl通讯建立起来之后，不考虑ssl版本上的一些漏洞，通信过程是很安全的，可以有效的防止中间人攻击和运营商劫持等问题，通讯数据的保密性和完整性得到了有效保证。所以针对SSL的相关攻击必须建立在其会话的初始化阶段，例如进行中间人攻击必须在会话建立过程中实现，即在通讯过程中攻击者伪造一个证书，对客户端声称该证书就是服务器证书来骗过客户端，使其信任该证书，并使用该证书来进行通讯，这样用户通讯的信息就会被第三监听，通讯保密性和完整性就被破坏。这种攻击方式也有一定的局限，当使用伪造的证书来建立通讯时，一般攻击者不会获得经过官方机构认证的证书，客户端的浏览器就会发过警告，提醒通信不安全，可以参考访问12306时的浏览器的提示（因为12306使用的是其自建的ca颁发的证书，没有经过官方机构的认证，所以浏览器就默认会认为其证书不安全，并提出告警）。



# 绕过浏览器校验

尽管通过浏览器去检验证书的合法性会提高攻击的成本，但在某些特殊情况下，还是会绕过浏览器的校验。例如：当我们使用fiddler和Charles这一类的抓包工具时，可以手动将他们的证书添加成为可信证书，所以当我们利用这些工具在本地对相关的系统进行逻辑分析时，并不会收到浏览器的告警。而且浏览器的校验仅仅针对的是web端的应用，对于移动端和其他非web端的应用只能通过其他方式来进行防护，一种比较通用的实现方式是使用SSL Pinning即证书绑定。

# SSL的工作原理中包含三个协议

## 1、握手协议

握手协议是客户端和服务器用于与SSL连接通信的第一个子协议。握手协议包括客户端和服务器之间的一系列消息。SSL中最复杂的协议是握手协议。该协议允许服务器和客户端相互进行身份验证，协商加密和MAC算法，以及保密SSL密钥以保护SSL记录中发送的数据。在应用程序的数据传输之前使用握手协议。

## 2、记录协议

在客户端和服务器握手成功之后使用记录协议，即客户端和服务器相互认证并确定安全信息交换使用的算法，并输入SSL记录协议，该协议为SSL提供两种服务连接：

 - **(1)保密性**：使用握手协议定义的秘密密钥实现
 - **(2)完整性**：握手协议定义了MAC，用于保证消息完整性

## 3、警报协议

客户机和服务器发现错误时，向对方发送一个警报消息。如果是致命错误，则算法立即关闭SSL连接，双方还会先删除相关的会话号，秘密和密钥。每个警报消息共2个字节，第1个字节表示错误类型，如果是警报，则值为1，如果是致命错误，则值为2;第2个字节制定实际错误类型。

# 证书的工作流程

1. 用户连接到你的Web站点，该Web站点受服务器证书所保护。(可由查看URL的开头是否为"https:"来进行辩识，或浏览器会提供你相关的信息)。

2. 你的服务器进行响应，并自动传送你网站的数字证书给用户，用于鉴别你的网站。

3. 用户的网页浏览器程序产生一把唯一的“会话钥匙码，用以跟网站之间所有的通讯过程进行加密。

4. 使用者的浏览器以网站的公钥对交谈钥匙码进行加密，以便只有让你的网站得以阅读此交谈钥匙码。

# 服务器端代码实现

```cpp
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <openssl/ssl.h>
#include <openssl/err.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>

#define CERT_FILE  "./cert/server.crt"
#define KEY_FILE   "./cert/server.key"

int main(int argc, char *argv[]) {
    SSL_CTX *ctx;
    int listenfd, newfd;
    struct sockaddr_in ser, cli;

    SSL_library_init();
    OpenSSL_add_all_algorithms();
    SSL_load_error_strings();
    if (NULL == (ctx = SSL_CTX_new(SSLv23_server_method()))) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }
    if (SSL_CTX_use_certificate_file(ctx, CERT_FILE, SSL_FILETYPE_PEM) != 1) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }
    if (SSL_CTX_use_PrivateKey_file(ctx, KEY_FILE, SSL_FILETYPE_PEM) != 1) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }
    if (1 != SSL_CTX_check_private_key(ctx)) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }

    listenfd = socket(AF_INET, SOCK_STREAM, 0);
    memset(&ser, 0, sizeof(ser));
    ser.sin_family = AF_INET;
    ser.sin_addr.s_addr = htonl(INADDR_ANY);
    ser.sin_port = htons(atoi(argv[1]));

    if (-1 == bind(listenfd, (struct sockaddr*)&ser, sizeof(struct sockaddr))) {
        perror("bind");
        exit(-1);
    }
    if (-1 == listen(listenfd, 5)) {
        perror("listen");
        exit(-1);
    }

    for (;;) {
        socklen_t size = sizeof(struct sockaddr);
        if (-1 == (newfd = accept(listenfd, (struct sockaddr*)&cli, &size))) {
            perror("accept");
            break;
        }
        SSL *ssl = SSL_new(ctx);
        SSL_set_fd(ssl, newfd);
        if (-1 == SSL_accept(ssl)) {
            ERR_print_errors_fp(stderr);
            close(newfd);
            break;
        }
        {
            char buf[256]; int len;
            while ((len = SSL_read(ssl, buf, sizeof(buf))) > 0) {
                buf[len] = 0;
                printf("%s", buf);
                SSL_write(ssl, buf, len);
            }
            SSL_shutdown(ssl);
            SSL_free(ssl);
            close(newfd);
        }
    }
    SSL_CTX_free(ctx);
    close(listenfd);
    return 0;
}
```
# 客服端代码实现

```cpp
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <openssl/ssl.h>
#include <openssl/err.h>
int main(int argc, char *argv[]) {
    SSL_CTX *ctx;
    int sockfd;
    struct sockaddr_in ser;

    SSL_library_init();
    OpenSSL_add_all_algorithms();
    SSL_load_error_strings();
    if (NULL == (ctx = SSL_CTX_new(SSLv23_client_method()))) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }

    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    memset(&ser, 0, sizeof(ser));
    ser.sin_family = AF_INET;
    ser.sin_addr.s_addr = inet_addr(argv[1]);
    ser.sin_port = htons(atoi(argv[2]));
    if (-1 == connect(sockfd, (struct sockaddr*)&ser, sizeof(ser))) {
        perror("connect");
        exit(-1);
    }
    SSL *ssl = SSL_new(ctx);
    SSL_set_fd(ssl, sockfd);
    if (-1 == SSL_connect(ssl)) {
        ERR_print_errors_fp(stderr);
        exit(-1);
    }
    {
        char buf[256]; int len;
        freopen("/etc/passwd", "r", stdin);
        while (fgets(buf, sizeof(buf), stdin)) {
            len = strlen(buf);
            if (SSL_write(ssl, buf, len) > 0) {
                buf[0] = 0;
                len = SSL_read(ssl, buf, sizeof(buf));
                if (len > 0) {
                    buf[len] = 0;
                    printf("%s", buf);
                }
            }
        }
        SSL_shutdown(ssl);
        SSL_free(ssl);
    }
    close(sockfd);
    SSL_CTX_free(ctx);
    return 0;
}
```
# 资源传送门

 - L关注【**做一个柔情的程序猿**】公众号
 - 在【**做一个柔情的程序猿**】公众号后台回复 【**python资料**】【**2020秋招**】 即可获取相应的惊喜哦！
 - 自己搭建的博客地址：[梦魇回生的博客](https://gain-wyj.cn/)

# 「❤️ 感谢大家」

 - 点赞支持下吧，让更多的人也能看到这篇内容（收藏不点赞，都是耍流氓 -_-）
 - 欢迎在留言区与我分享你的想法，也欢迎你在留言区记录你的思考过程