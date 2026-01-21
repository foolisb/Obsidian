1## [TLS 1.2](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=TLS+1.2&zhida_source=entity) 握手过程（经典握手）

### 1. [Client Hello](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=Client+Hello&zhida_source=entity)

客户端发送初始消息，包含

- 支持的TLS协议版本（如TLS 1.2）
- 客户端生成的随机数（Client Random）
- 支持的加密套件列表（Cipher Suites）
- 支持的压缩方法
- SNI（Server Name Indication）扩展，指明目标服务器

### 2. [Server Hello](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=Server+Hello&zhida_source=entity)

服务器响应，包含

- 选择的TLS协议版本
- 服务器生成的随机数（Server Random）
- 选择的加密套件
- 选择的压缩方法

### 3. 证书传输

服务器发送[证书链](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=%E8%AF%81%E4%B9%A6%E9%93%BE&zhida_source=entity)（Certificate）

- 包含服务器的数字证书
- 可能包含中间证书颁发机构（CA）的证书
- 证书中包含服务器的公钥

### 4. 服务器密钥交换（可选）

- 对于某些密钥交换算法（如DHE、[ECDHE](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=ECDHE&zhida_source=entity)），服务器发送额外参数
- 这些参数用于完善前向保密（Perfect Forward Secrecy）机制

### 5. 服务器Hello完成

- 服务器发送ServerHelloDone消息，表示服务器的初始协商消息已发送完毕

### 6. 客户端密钥交换

- 客户端生成[预主密钥](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=%E9%A2%84%E4%B8%BB%E5%AF%86%E9%92%A5&zhida_source=entity)（Pre-master Secret）
- 使用服务器公钥加密预主密钥
- 将加密后的预主密钥发送给服务器

### 7. 密钥生成

双方使用相同的算法，基于三个输入计算主密钥（Master Secret）

- 预主密钥（Pre-master Secret）
- 客户端随机数（Client Random）
- 服务器随机数（Server Random）

从主密钥派生出会话密钥，包括：

- 客户端写入密钥
- 服务器写入密钥
- 客户端MAC密钥
- 服务器MAC密钥
- 初始化向量（如果需要）

### 8. 变更密码规范

- 客户端发送[ChangeCipherSpec](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=ChangeCipherSpec&zhida_source=entity)消息，表示后续通信将使用协商好的密钥和算法
- 服务器也发送ChangeCipherSpec消息

### 9. 完成消息

- 客户端发送[Finished](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=Finished&zhida_source=entity)消息，包含之前所有握手消息的哈希值
- 服务器验证此消息，确保握手没有被篡改
- 服务器也发送Finished消息
- 客户端验证此消息

### 10. 应用数据传输

- 握手完成后，客户端和服务器使用协商好的会话密钥进行加密通信

## [TLS 1.3](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=TLS+1.3&zhida_source=entity) 握手过程（简化握手）

TLS 1.3显著简化了握手过程，减少了往返次数

### 1. Client Hello

除了TLS 1.2中的信息外，客户端还包含

- 猜测服务器可能使用的密钥共享参数
- 支持的签名算法
- 提前发送的密钥共享参数（如ECDHE参数）

### 2. Server Hello

服务器回应，包含

- 选定的密码套件（TLS 1.3仅支持AEAD加密套件）
- 服务器的密钥共享参数

### 3. 加密扩展

服务器立即发送加密的扩展数据，包括

- 证书（Certificate）
- 证书验证（CertificateVerify）
- 完成消息（Finished）

### 4. 客户端确认

- 客户端发送加密的完成消息（Finished）

### 5. 应用数据传输

- 握手在1-RTT（一个往返时间）内完成
- 在某些情况下，客户端可以在第一个消息中包含应用数据（0-RTT数据）

## 重要的安全机制

**前向保密**：使用临时密钥（如ECDHE）确保即使长期私钥泄露，之前的通信仍然安全

**会话恢复**：

- **会话ID**：服务器存储会话状态，客户端提供ID以恢复
- **会话票证**：服务器加密会话信息，客户端在以后提供此票证以恢复

**证书验证**：

- 验证证书链
- 检查证书有效期
- 检查证书吊销状态（CRL或OCSP）
- 验证证书中的域名与目标服务器匹配

**TLS扩展**

- SNI（Server Name Indication）：在共享IP上支持多个HTTPS站点
- ALPN（Application-Layer Protocol Negotiation）：协商应用层协议（如HTTP/2）
- [OCSP Stapling](https://zhida.zhihu.com/search?content_id=255035849&content_type=Article&match_order=1&q=OCSP+Stapling&zhida_source=entity)：服务器提供预先获取的OCSP响应

## 示意图

![[Pasted image 20260121154755.png]]

1.2

![[Pasted image 20260121154801.png]]

1.3

[编辑于 2025-03-13 21:12](https://zhuanlan.zhihu.com/p/30067960536)