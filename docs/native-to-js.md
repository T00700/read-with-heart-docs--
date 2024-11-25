# Native to Javascript
App暴露的Javascript方法，可以使用这些方法调用原生的组件！

## log
打印调试日志，等同于`console.log()`

=== "示例"

    ```javascript
    App.log('你好!', 'hello!')
    ```
=== "API"
    
    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | args | 参数 | Any | - | 支持多个参数 |
    
## toast

提示用于需要用户注意的信息。

=== "示例"

    ```javascript
    App.toast('这是一个提示！', 'info')
    ```
=== "API"

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | info | 信息 | String | - | 需要提示的信息 |
    | type | 提示类型 | String | `info` | info/success/error |

## showDialog
在APP中打开一个对话框承载相应的信息及操作。
=== "示例"

    ```javascript linenums="1"
    const params = {
        title: '标题',
        body: '提示的内容',
        confirmBtn: '确认',
        cancelBtn: false // (1)!
    }

    App.showDialog(params, (bool) => {
        console.log(bool)
    })
    ```

    1. 取消按钮如果传入false,则会隐藏
=== "API"

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | params | 参数 | Object | `{}` | 参数 |
    | - `title` | 标题名称 | String | `提示` | - |
    | - `body` | 提示内容 | String | - | - |
    | - `confirmBtn` | 确认按钮名称 | String | `确认` | - |
    | - `cancelBtn` | 取消按钮名称 | String/Boolean | `取消` | 如果传入false，则会隐藏该按钮. |
    | onCompletion | 回调方法 | function | - | 点击确认按钮返回true,取消按钮返回false |

## isJson

判断一个字符串是否为json格式。

=== "示例"

    ```javascript
    let bool = App.isJson('{"a":"b"}') // return true
    let bool = App.isJson('你好') // return false
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | jsonString | 字符串 | String | - | 需要判断的字符串 |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | bool | 状态 | Boolean | true/false |

## jsonToString

将一个json对象转换成一个json字符串

=== "示例"

    ```javascript
    let jsonObject = {
        a: '1',
        b: '2'
    }
    let jsonString = App.jsonToString(jsonObject)
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | json | json | Object | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | jsonString | - | string | 转换失败返回`null` |

## stringToJson

将一个json字符串转换成一个json对象

=== "示例"

    ```javascript
    let jsonString = '{"a":"1","b":"2"}'
    let jsonObject = App.stringToJson(jsonString)
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | jsonString | json字符串 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | jsonObject | - | Object | 转换失败返回`null` |

## time

将一个UNIX时间戳转换为日期格式.

=== "示例"

    ```javascript
    let unix = '1732340412'
    let formatDate = App.time(unix, 'yyyy-MM-dd HH:mm:ss')
    App.log(formatDate) // 2024-11-23 13:40:12
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | unix | 时间戳 | String/Number | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | string | - | String | 转换失败返回`null` |

## post/get

进行一个HTTP的POST/GET请求，采用原生交互写法，不存在普通js的ajax跨域问题.

=== "示例"

    ```javascript linenums="1"
    // 请求地址
    let url = 'your url'

    // 请求参数
    let params = {
        type: 1
    }
    
    // header信息
    let header = {
        token: 'xxxx'    
    }
    
    // 开始请求，采用 async 方式，所以必须带修饰符 await
    let data = await App.post({
        url,
        params,
        header
    });
    
    // GET请求，params中的参数会自动和url拼接，即http://www.a.com/?type=1
    let data = await App.get({
        url,
        params,
        header
    });

    App.log(data)
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | params | 参数 | Object | - | - |
    | - `url` | 请求地址 | String | - | - |
    | - `params` | post请求参数 | Object | `{}` | - |
    | - `header` | header信息 | Object | `{}` | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | responseBody | - | String | 响应的内容 |
    | requestHeader | - | Object | 请求标头 |
    | responseHeader | - | Object | 响应标头 |
    | cookie | - | Object | Cookie |

## 简易存储

将一个或多个参数本地化保存,获取和删除.

### put
持久化数据

=== "示例"

    ```javascript
    let bool = App.sp.put('name', 'zhangsan');
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | key | 键 | String | - | - |
    | value | 值 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | bool | - | Boolean | 写入成功返回true |

### get
获取本地持久化的数据

=== "示例"

    ```javascript
    let data = App.sp.get('name');
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | key | 键 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | data | - | String | 返回存入的数据 |

### delete
删除一个本地持久化的数据

=== "示例"

    ```javascript
    let bool = App.sp.delete('name');
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | key | 键 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | bool | - | Boolean | 删除本地的数据 |

## MD5 加密

一种被广泛使用的密码散列函数，用于确保信息传输完整一致。

=== "示例"

    ```javascript
    let data = 'Hello, World!';
    
    // 加密
    let encryptData = App.md5(data);
    App.log(encryptData); // 65a8e27d8879283831b664bd8b7f0ad4
    ```

=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | data | 加解密内容 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | data | - | String | 密文 |


## AES 加解密

AES（高级加密标准） 是一种对称加密算法，广泛应用于数据加密和保护数据隐私。AES 的加密和解密使用相同的密钥，因此加密和解密过程对发送方和接收方来说是对称的。AES 算法支持多种密钥长度，包括 128 位、192 位和 256 位。

=== "示例"

    ```javascript
    let data = "Hello, World!"
    let key = "1234567890123456" // 16 字节的密钥，128 位
    let iv = "1234567890123456" // 初始化向量（IV）
    
    // 加密
    let encryptData = App.aes.encrypt(data, key, iv);

    // 解密
    let decryptData = App.aes.decrypt(encryptData, key, iv);
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | data | 加解密内容 | String | - | - |
    | key | 密钥 | String | - | - |
    | iv | 初始化向量 | String | - | - |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | data | - | String | Base64后的密文/解密内容 |


## RSA 加解密

RSA（Rivest-Shamir-Adleman）是一种广泛使用的 非对称加密算法，它使用一对密钥：公钥和私钥。公钥用来加密数据，私钥用来解密数据。与对称加密（如 AES）不同，非对称加密使用不同的密钥进行加密和解密，因此更适合用于安全的通信和数据交换。

=== "示例"

    ```javascript
    let data = "Hello, World!"
    let publicKey = "公钥密钥串"
    let privateKey = "私钥密钥串"
    
    // 加密
    let encryptData = App.rsa.encrypt(data, publicKey);

    // 解密
    let decryptData = App.rsa.decrypt(encryptData, privateKey);
    ```
=== "API"

    - 请求

    | 参数 | 名称 | 类型 | 默认值 | 说明 |
    |:---|:---|:---|:---|:---|
    | data | 加解密内容 | String | - | - |
    | publicKey/privateKey | 密钥 | String | - | 加密用公钥，解密用私钥 |

    - 响应

    | 参数 | 名称 | 类型 | 说明 |
    |:---|:---|:---|:---|
    | data | - | String | 密文/解密内容 |