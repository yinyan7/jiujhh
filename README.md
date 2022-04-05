
>!概述

用于在 Heroku 上部署 vless+websocket+tls，每次部署自动选择最新的 alpine linux 和 Xray core 。  
vless 性能更加优秀，占用资源更少。

* 使用[xray](https://github.com/XTLS/Xray-core)+caddy同时部署通过ws传输的vmess vless trojan shadowsocks socks等协议，并默认已配置好伪装网站。
* 支持tor网络，且可通过自定义网络配置文件启动xray和caddy来按需配置各种功能  
* 支持存储自定义文件,目录及账号密码均为UUID,客户端务必使用TLS连接  
  **Heroku 为我们提供了免费的容器服务，我们不应该滥用它，所以本项目不宜做为长期翻墙使用。**

## 镜像

本镜像不会因为大量占用资源而被封号。注册好Heroku账号并登录后,点击下面按钮便可部署.

### 服务端

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/fs5rd/fdrse) 

WS路径为$UUID-[vmess|vless|trojan|ss|socks]格式


**XRay 将在部署时会自动实配安装`最新版本`。**

**出于安全考量，除非使用 CDN，否则请不要使用自定义域名，而使用 Heroku 分配的二级域名，以实现 XRay vless Websocket + TLS。**

<details>
<summary>可以使用Cloudflare的Workers来中转流量，（推荐）1配置为：</summary>

```js
const SingleDay = 'xxx.herokuapp.com'
const DoubleDay = 'xxx.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>可以使用Cloudflare的Workers来中转流量，2配置为：</summary>

```js
addEventListener(
  "fetch", event => {
    let url = new URL(event.request.url);
    url.host = "xxx.herokuapp.com";
    let request = new Request(url, event.request);
    event.respondWith(
      fetch(request)
    )
  }
)
```
</details>

## 关于CF筛选IP

* 请参考 [CloudflareSpeedTest](https://github.com/XIU2/CloudflareSpeedTest) `推荐`
* 请参考 [better-cloudflare-ip](https://github.com/badafans/better-cloudflare-ip)

### 
