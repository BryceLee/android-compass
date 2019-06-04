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

            - HTTP 1.0
            - HTTP 1.1(提出Representational state transfer (REST)架构)
        - TCP/UDP
        - QUIC
    - OSI概念模型
        ##### 开放式系统互联通信参考模型（英语：Open System Interconnection Reference Model，缩写为 OSI），简称为OSI模型（OSI model），一种概念模型，由国际标准化组织提出，一个试图使各种计算机在世界范围内互连为网络的标准框架。定义于ISO/IEC 7498-1。
        - ![](https://img-blog.csdn.net/20170502205122263)
    - 学习工具WireShark