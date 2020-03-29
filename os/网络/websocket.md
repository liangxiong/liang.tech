# websocket

## 前提：
- 通过HTTP协议进行握手
- 同一个端口能够接受HTTP客户端和WebSocket客户端
- WebSocket客户端的握手是HTTP请求的升级

## 报文：
#### 请求
GET / HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Host: example.com
Origin: http://example.com
Sec-WebSocket-Key: sN9cRrP/n9NdMgdcy2VJFQ==
Sec-WebSocket-Version: 13

- Sec-WebSocket-Key：随机字符串的BASE-64编码
- Sec-WebSocket-Version：必须13   

#### 返回
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: fFBooB7FAkLlXgRSz0BT3v4hq5s=

- Sec-WebSocket-Accept
  - Sec-WebSocket-Key + 258EAFA5-E914-47DA-95CA-C5AB0DC85B11
  - 计算SHA-1摘要
  - 进行BASE-64编码
