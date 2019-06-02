## Postman使用RSA加密
### 场景1  
> 加密用户名和密码  

- 获取加密js  

    ```shell
    git clone git@github.com:digitalbazaar/forge.git
    cd forge
    npm install --registry=http://registry.npm.taobao.org
    webpack.config.js并将文件umd替换为var
    npm run build
    cp dist/forge.js 到nginx服务器上或者其他web服务器，并记录http请求地址

    ```


- pre-scripts  

```javascript
//公钥，使用forge.js需要在公钥前后添加BEGIN PUBLIC KEY 和 END PUBLIC KEY
var pem="-----BEGIN PUBLIC KEY-----\n"+
"MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAObWjDUTnjwFhXT5OqOMhxoJkhMFBCHiI2q+MPNeo6VI1HLb26H0E8pt17cQciASM0lNF9V0C/yfR4mI/JDa098CAwEAAQ=="+"\n"+
"-----END PUBLIC KEY-----";

//待加密的数据
var userCode = "admin";
var password="admin";

//发送请求，获取forge.js, sendRequest请求默认为异步请求
pm.sendRequest({

    url:"http://localhost:8080/forge.js",
    method:"GET",
    header:{
        "Content-Type":"javascript/json;charset=UTF-8"
    }
}, function (err,res) {
  //请求成功，加载js文本
    eval(res.text());

    var pki = forge.pki;
    // convert a PEM-formatted public key to a Forge public key
    //获得公钥
    var publicKey = pki.publicKeyFromPem(pem);
    //获取用户名的二进制数据
    var buffer = forge.util.createBuffer(userCode,'utf8');
    var bytes = buffer.getBytes();
    //加密用户名
    var userCode_encrypt = forge.util.encode64(publicKey.encrypt(userCode,'RSAES-PKCS1-V1_5',{
         md: forge.md.sha256.create(),
         mgf1: {
            md: forge.md.sha1.create()
        }
    }));
    //密码加密
    buffer = forge.util.createBuffer(password, 'utf8');
    bytes = buffer.getBytes();
    var password_encrypt = forge.util.encode64(publicKey.encrypt(password, 'RSAES-PKCS1-V1_5', {
        md: forge.md.sha256.create(),
        mgf1: {
            md: forge.md.sha1.create()
        }
    }));
   //控制台打印
    console.log(userCode_encrypt);
    console.log(password_encrypt);
    //封装结果
    var result = "{'userCode':'"+userCode_encrypt+"','password2':'"+password_encrypt+"'}";
    //设置全局变量
    pm.globals.set("encryptData",result);
});

```
- body
  - raw->application/json
  ```shell
  #使用postman设置的环境变量的值
  {{encryptData}}
  ```
- tests  
> 断言判断和一些响应完成处理
  ```shell
pm.globals.unset("encryptData");
  ```
