- 计算机网络
    - 网络协议
        #### 这里需要注意RFC文档， 它是用来记录互联网规范、协议、过程等的标准文件。基本的互联网通信协议都有在RFC文件内详细说明。
        #### 请求意见稿（英语：Request For Comments，缩写：RFC）是由互联网工程任务组（IETF）发布的一系列备忘录。文件收集了有关互联网相关信息，以及UNIX和互联网社群的软件文件，以编号排定。当前RFC文件是由互联网协会（ISOC）赞助发行。
        - Http
            - The Hypertext Transfer Protocol (HTTP) is a stateless application-
   level protocol for distributed, collaborative, hypertext information
   systems.  This document provides an overview of HTTP architecture and
   its associated terminology, defines the "http" and "https" Uniform
   Resource Identifier (URI) schemes, defines the HTTP/1.1 message
   syntax and parsing requirements, and describes related security
   concerns for implementations.[(FROM FRF74230)](https://tools.ietf.org/html/rfc7230),
            - 超文本传输协议是为分布式的，协作的，超文本信息系统而生的无状态，应用层的协议。他采用request/response的方式来进行客户端和服务器之间的通信。
            - 请求和响应由请求行(状态行)+头部+请求体(响应体)构成。
            - 用ABDF规范来HTTP的形式
                - 扩充巴科斯-瑙尔范式[(ABNF)](https://zh.wikipedia.org/wiki/%E6%89%A9%E5%85%85%E5%B7%B4%E7%A7%91%E6%96%AF%E8%8C%83%E5%BC%8F)是一种基于巴科斯-瑙尔范式（BNF）的元语言，但它有自己的语法和派生规则。描述一种作为双向通信协议语言的形式系统.
                - 操作符（定义语法）
                    - 空白字符SP
                    - 选择/
                    - 值范围%c##-##
                    - 不定量重复a*b
                    - 可选字符[]:
                    - [message-body]
                - 核心规则
                    - CR (换行)
                    - LF（换行）
                    - HTAB （横向字表符号）
                - ABDF描述的HTTP协议格式：
                   
                    - HTTP-meaage=startline *(header-field CRLF)CRLF[message-body]
                        - start-line = method SP request/response request-target SP HTTP-version CRLF
                        - status-line = HTTP-version SP status-code SP reason-phrase CRLF 
                        - filed-value=*(field-content/obs-fold)
                        - message-body=*OCTET
            - HTTP 0.9
                - Request-Response模式
                - 只支持GET方法，不支持请求头。
            - HTTP 1.0
                - 扩充了0.9
                - 增加Header
                - 增加State Code
            - HTTP 1.1(提出Representational state transfer (REST)架构)
                - 设置keepalive可以让HTTP重用TCP链接，进而省了每次请求都要进行TCP三次握手的开销（HTTP Persistent Connection）
                - 增加Cache Control
                - 协议头增加Language,Encoding,Type,Host
                - 加入TLS协议加大安全性
                - 支持四种网络协议
                    - 短链接
                    - 重用TCP长链接
                    - 服务端PUSH模型
                        - 支持Chunked Response.在Response中不必说明Content-Length。客户端就不断连接，直到收到服务端的EOF标识。
                    - WebSocket模型
            - HTTP 2
                - 把请求从串行改成并行，增加了更大的网络吞吐量和性能
                - 传输数据从文本协议改成二进制协议，提高传输效率(文本协议需要用ZIP压缩，占用客户端和服务端更多CPU资源)
                - 会压缩头，多个请求头相似会去重（HPACK算法）
                - 允许服务端在客户端防缓存
            - HTTP 3
                - HTTP2的问题：多请求复用一个TCP连接，发生丢包，所以请求被阻塞
                - 解决：把TCP改成UDP（UDP不管顺序和丢包）
        - TCP/UDP
            - TCP协议采用three-way handshaking策略
                - Flag:
                    - SYN(Sychronized)
                    - ACK(Acknowledgement)
        - QUIC（Quick UDP Internet  Connections）
            - 基于UDP，在HTTP3中，解决HTTP2的多请求复用同一个TCP，丢包阻塞所有请求的问题
            - HTTPS连接下，合并TCP的三次握手和TLS的三次握手
            ![](https://coolshell.cn/wp-content/uploads/2019/10/http-request-over-tcp-tls@2x-292x300.png)![](https://coolshell.cn/wp-content/uploads/2019/10/http-request-over-quic@2x-300x215.png)
    - OSI概念模型
        ##### 开放式系统互联通信参考模型（英语：Open System Interconnection Reference Model，缩写为 OSI），简称为OSI模型（OSI model），一种概念模型，由国际标准化组织提出，一个试图使各种计算机在世界范围内互连为网络的标准框架。定义于ISO/IEC 7498-1。
        - ![](https://img-blog.csdn.net/20170502205122263)
    - 学习工具WireShark
    - Web架构关键属性
        - Performance
            - Network Performance
            - User-perceived Performance
            - Network Efficiency
                - Cache（更少的交互次数）
                - CDN（更短的传输路径）
        - Scalability（可伸缩性）
        - Simplicity
        - Modifiability(可修改性)
            - Evolvability（可进化性）
            - Customizability（可定制性）：定制提供服务，不对常规用户产生影响
            - Configurability（可配置性）：应用部署后可以通过修改配置来提供新的功能
            - Reusability（可重用性）：组建可以不做修改在其他应用中使用

# Thanks
- https://zh.wikipedia.org/wiki
- https://github.com/russelltao/geektime-webprotocol
- [HTTP的前世今生](https://coolshell.cn/articles/19840.html)