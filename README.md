# WeChatPad-Docker

WeChatPadPro 是基于 WeChat Pad 协议的高级微信管理工具，支持以下功能：

- 朋友圈收发与互动（点赞、评论）
- 好友管理（添加、删除、清理僵尸粉）
- 消息收发（文本、图片、名片、动图、文件）
- 微信支付（转账、红包）
- 小程序和名片分享
- 通讯录好友添加
- 微信收藏
- 标签管理

此外，还支持强大的群管理功能，包括：

- 消息群发
- 自动通过好友请求
- 建群、拉人进群、踢群成员、邀请成员、退群
- 修改群名称、群公告发布
- 多群消息同步等

WeChatPadPro 适用于个人微信增强、运营管理和自动化交互，提升微信使用效率和管理能力。

## 联系我们

如有任何问题或建议，欢迎通过 GitHub Issues 或邮件与我们联系。

---




# 忠告: 切记莫贪,新号尽量稳定挂机 3 天后再使用(危险性高的API操作),过来人的忠告

> 注意看下面的[关于风控](##关于风控)问题；



# 异地登录一定要设置同市Socks5代理



> 注意看下面的[关于风控](##关于风控)问题；
>
> 代理链接格式：`socks5://用户名:密码@代理IP:代理端口`
>
> 尽量找同市IP，没有可以用同省IP；
>
> 新号首次登录时(同省IP首次可能会多次掉线, 同市掉线少, 家里的内网穿透 socks5 代理IP基本不会掉线)；
>
> 不会搭建家里的内网穿透 socks5 代理的可联系我搭建；
>
> frp：https://github.com/fatedier/frp/releases；



# 新号首次登录时,可能立即掉线,多扫码登录两次即可稳定;另本服务内部自动保持登录心跳



> 注意看下面的[关于风控](##关于风控)问题；
>
> 新号登录后，24小时内可能还会掉线一次(如下图)，再次登录即可(登录时使用原来的Api `key`，不要切换新的，更换`key`相当于新设备登录了)；再次登录后基本3个月内不会掉线；
>
> 注意⚠️：一个授权码`key`只能给一个微信号使用，多个号请生成多个授权码`key`；
>
> 3天后基本稳定，7天后更稳了；

![](./static/doc/logout_error.png)





---
## 详细的api文档
如果要尝试调用，请查阅以下文档
[wechatpadproAPI文档](https://doc.apipost.net/docs/460ada21e884000?locale=zh-cn  )




# 食用方法

```shell
docker compose up -d
# or
docker-compose up -d
```
> dockercompose中的mysql和redis密码等配置应与setting.json一致
> app/assets/setting.json


## 软件配置

> `assets/setting.json`：全局配置
>
> `assets/owner.json`：管理员/所有者 配置



### setting.json

> 你能修改的字段如下，其他字段**不用修改！不用修改！**；
>
> - debug：是否开启debug日志；
> - port：当前服务端口号；
> - apiVersion：当前服务API版本，例如这里的`/v849`就是API版本，`http://127.0.0.1:8848/v849`；可以设为空简化URL，此时服务BASE_URL为`http://127.0.0.1:8848`；
> - ghWxid：要引流关注的微信公众号的wxid；新用户登录时自动关注；默认为空，不关注任何公众号；
> - adminKey：管理相关接口(例如`GenAuthKey`等接口)的授权KEY，若留空每次服务启动随机生成；
> - redisConfig.Port：Redis服务端口号；
> - redisConfig.Db：要使用的Redis几号数据库；
> - redisConfig.Pass：Redis服务密码；
> - mySqlConnectStr：`用户名:密码@tcp(127.0.0.1:3306)/数据库名?charset=utf8mb4&parseTime=true&loc=Local`；

```json
{
  "debug": false,
  "host": "0.0.0.0",
  "port":"8848",
  "apiVersion": "/v849",
  "ghWxid": "",
  "adminKey": "",
  "redisConfig": {
    "Host":            "127.0.0.1",
    "Port":            6379,
    "Db":              1,
    "Pass":           "12345678"
  },
  "mySqlConnectStr": "wechat_mmtls:12345678@tcp(127.0.0.1:3306)/wechat_mmtls?charset=utf8mb4&parseTime=true&loc=Local"
}

```



### owner.json

> 这里要设置管理员微信号的`wxid`，注意是`wxid`，不要设置错了；
>
> 这里设置管理员`wxid`后，管理员扫码登录后，可以使用微信的`文件传输助手使用部分命令管理；

```json
{
  "wxid_xxx": 1
}
```





## 启动教程

1. 修改基础设置 [setting.json](./assets/setting.json) 

   设置你自己的`adminKey`或留空随机，修改mysql 与 redis的连接地址、账户名、密码等信息；

2. 修改管理员设置 [owner.json](./assets/owner.json) 

   添加你的 `wxid`，注意是`wxid`，别填错了；

3. docker compose up -d   

4. 获取全程操作的AuthKey：http://127.0.0.1:8848/v849/login/GenAuthKey2?key=ADMIN_KEY&count=1&days=365 (生成 `count=`1 个有效期为 `days=`365 的API授权码)；

   注意：服务Owner(超级管理员)也可以在微信的`文件管理助手`生成key；

5. 登录：

   - 1、获取二维码，传家附近的代理：`socks5://用户名:密码@代理IP:代理端口`
   - 2、获取二维码状态



## 关于风控
以下图片来自网络,并非本项目,仅供参考



![](./static/doc/img_1.png)



![](./static/doc/img.png)





## 关于测试

> 可下载 [ApiPOST(v7.2.X)经典版！经典版！](https://www.apipost.cn/download.html) ，
>
> 
>
> 之后，必须要设置以下环境变量：
>
> - `WS_URL`：你的WX-Web-API服务的WebSocket基础URL，例如 `ws://127.0.0.1:8848/v849`；
> - `ADMIN_KEY`：请求`/login/GenAuthKey`、`/login/GenAuthKey2`接口所需的管理接口KEY；
> - `SOCKS5`：socks5代理；最好是家附近的代理IP，其次同市IP，最其次同省IP；异地IP极易风控；
>
> 
>
> 之后，请求`/login/GenAuthKey`或`/login/GenAuthKey2`接口，将生成的UUID保存为`TOKEN_KEN`环境变量即可，之后所有WX接口操作均会携带该值；
>
> 
>
> 另外注意：这些ApiPOST接口定义里面，部分已经内置好了【请求的后置处理操作】---【自定义处理resopnse响应的脚本】，会自动提取并设置某些环境变量的值；
>
> 



![](./static/doc/apipost1.png)

![](./static/doc/apipost2.png)





