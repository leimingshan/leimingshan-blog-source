---
title: JavaScript RSA encrypt 公钥加密
category: 开发
tags:
  - JavaScript
  - NodeJs
  - Java
abbrlink: 725aae6c
date: 2018-10-19 11:30:57
---
在前端与后端交互的过程中，很可能会遇到使用RSA公钥加密，然后用私钥解密的情况，
RSA的私钥签名，公钥验签同样也很常用，这里简单介绍一下 JavaScript 语言RSA算法的一些解决方案。

# NodeJs环境
可以直接引入NodeJs自带的crypto模块，基于RSA/ECB/PKCS1Padding加密，用法如下：
```
const crypto = require('crypto');
private RSAEncrypt(rsakey:string, text:string){
    let encrypted = crypto.publicEncrypt({key:rsakey,
        padding:crypto.constants.RSA_PKCS1_PADDING},new Buffer(text)).toString('base64');
    return encrypted;
}
```
其中rsakey为pem格式的公钥，可以从文件读取，或者指定字符串，字符串格式如下：
```
rsakey='-----BEGIN PUBLIC KEY-----\nMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCm5IN1uRvak0Kod3YviD/b67dj\nzP/ubU+8RgbCnm1HUVYBAVGnvg5epMfGunXKFXSb1ehOvQ2K+fEJLa+pKy2uLZLd\n/6gbUGJn+q8wGiFKfu0U0H3E+2yH6eFX+IPXx5OJNwUE6yqKR6hOBz5qR/AtVRfM\n6aAcDLIR7wE06SnHVQIDAQAB\n-----END PUBLIC KEY-----\n'
```

text就是准备加密的字符串了，直接调用该方法就可以完成加密过程。

参考：https://nodejs.org/docs/latest-v8.x/api/crypto.html#crypto_crypto_publicencrypt_key_buffer

# JavaScript native or a web browser
如果是单纯的浏览器环境，推荐[forge.js](https://github.com/digitalbazaar/forge)。
代码示例：
```
<!doctype html>
<html>
  <head>
    <title>JavaScript RSA Encryption</title>
    <script src="http://code.jquery.com/jquery-1.8.3.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/node-forge@0.7.0/dist/forge.min.js"></script>
    <script type="text/javascript">

      // Call this code when the page is done loading.
      $(function() {

        // Run a quick encryption/decryption when they click.
        $('#testme').click(function() {

          var publicKey = forge.pki.publicKeyFromPem($('#pubkey').val());

          // convert string to UTF-8 encoded bytes
          var buffer = forge.util.createBuffer($('#input').val());
          var bytes = buffer.getBytes();

          // encrypt data with a public key using RSAES PKCS#1 v1.5
          var encrypted = publicKey.encrypt(bytes, 'RSAES-PKCS1-V1_5');

          // base64-encode encrypted data to send to server
          var b64Encoded = forge.util.encode64(encrypted);
          console.log(b64Encoded);
          alert(b64Encoded);
        });
      });
    </script>
  </head>
  <body>
    <label for="pubkey">Public Key</label><br/>
    <textarea id="pubkey" rows="15" cols="65">-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDY08fkt9d32KHV1JapaZGtk+fL
a7wvxMAdZ0oENhx6HgBisSlDwCgBfRtN5l2iOjKm+eByp7Od24JqIOwgDF6FZ/ol
CQbnXjzPhG0EOeLnu3AXntzhu9A0yS7b3p+pxGR7EvhIz1xkOS4bNABsAgF7Y5Zu
B1RpsOZZdxNHGeStBwIDAQAB
-----END PUBLIC KEY-----</textarea><br/>
    <label for="input">Text to encrypt:</label><br/>
    <textarea id="input" name="input" type="text" rows=4 cols=70>Hello World!</textarea><br/>
    <input id="testme" type="button" value="RSA Encrypt" /><br/>
  </body>
</html>
```

参考：
- https://github.com/digitalbazaar/forge
- https://github.com/digitalbazaar/forge/issues/407

<!--more-->

# Java
最后以我最熟悉的JAVA举例，直接使用java.security的几个相关类即可，列举几个关键的方法吧。
```
public static final String CIPHER_TRANSFORMATION = "RSA/ECB/PKCS1Padding";
public static final String CIPHER_ALGORITHM = "RSA";

Cipher cipher = Cipher.getInstance(CIPHER_TRANSFORMATION);
cipher.init(Cipher.ENCRYPT_MODE, getRSAPublicKeyFromStr(publicKeyStr));
byte[] encryptedData = cipher.doFinal(data);

String encryptedStr = Base64.getEncoder().encodeToString(encryptedData);
```
