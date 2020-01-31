# Base64
- Java类库的标准
- 采用了A-Z a-z 0-9  +  / 垫字符串= 这64个字符来编码原始字符
- 一个字符本身是一个字节，也就是8位
  - base64编码一个字符只能表示6位的信息。原始字符串中的3字节的信息编码会变成4字节的信息
- Base64的主要作用是满足MIME的传输需求

## 三种BASE64编解码器
- 基本：输出被映射到一组字符A-Za-z0-9+/，编码不添加任何行标，输出的解码仅支持A-Za-z0-9+/。
- URL：输出映射到一组字符A-Za-z0-9+_，输出是URL和文件。
  - '+'  '/'  改为 '-' '_'
- MIME (Multipurpose Internet Mail Extensions)：输出隐射到MIME友好格式。
  - 输出每行不超过76字符
  - 并且使用'\r\n'作为分割。编码输出最后没有行分割

## 常用方法：
### 编码器：
- Base64.getEncoder()
- Base64. getUrlEncoder()
- Base64.getMimeEncoder()
- 方法
  - byte[] encode(byte[] src)
  - int encode(byte[] src, byte[] dst)
  - String encodeToString(byte[] src)
  - ByteBuffer encode(ByteBuffer buffer)
  - OutputStream wrap(OutputStream os)
  - Encoder withoutPadding()

### 解码器
- Base64.getDecoder()
- Base64.getUrlDecoder()
- Base64.getMimeDecoder()
- 方法
  - byte[] decode(byte[] src)
  - byte[] decode(String src)	解
  - int decode(byte[] src, byte[] dst)
  - ByteBuffer decode(ByteBuffer buffer)
  - InputStream wrap(InputStream is)
