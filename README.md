 申请Freenom域名的正确做法  https://1drv.ms/w/s!Akco49yyqyqDtFXjm4iy26Mvv-ES?e=cCC6Wl

 CloudFlare Workers 被墙解决方案  https://1drv.ms/w/s!Akco49yyqyqDtFK69rvLkNevKxyY?e=HegeOL

 Heroku搭建教程  https://1drv.ms/w/s!Akco49yyqyqDtFOlWqW7k2FHnBnG?e=Q4sZl2


 讨论群组  https://t.me/herokuvless

## v2ray-heroku

[![](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/fanyuhsin/Herov2.git)https://heroku.com/deploy?template=

## 注意事项
### heroku上部署v2ray
- [x] 支持VMess和VLESS两种协议
- [x] 使用v2ray最新版构建




### 环境变量说明

|  名称 | 值  | 说明  |
| ------------ | ------------ | ------------ |
|  PROTOCOL |  vless<br>vmess（可选） |  协议：vless+ws+tls或vmess+ws+tls |默认vless
|  UUID |  [uuid在线生成器](https://www.uuidgenerator.net "uuid在线生成器") | 用户主ID  |


### CloudFlare Workers反代代码
<details>
<summary>CloudFlare Workers单账户反代代码</summary>

```js
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers单双日轮换反代代码</summary>

```js
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
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
<summary>CloudFlare Workers每五天轮换一遍式反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
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
<summary>CloudFlare Workers一周轮换反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
const Day5 = 'app5.herokuapp.com'
const Day6 = 'app6.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
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
<summary>CloudFlare Pages单账户反代代码</summary>

```js
export default {
  async fetch(request, env) {
    let url = new URL(request.url);
    if (url.pathname.startsWith('/')) {
      url.hostname = 'app0.herokuapp.com'
      let new_request = new Request(url, request);
      return fetch(new_request);
    }
    return env.ASSETS.fetch(request);
  },
};
```
</details>

<details>
<summary>CloudFlare Pages单双日轮换反代代码</summary>

```js
export default {
  async fetch(request, env) {
    const day1 = 'app0.herokuapp.com'
    const day2 = 'app1.herokuapp.com'
    let url = new URL(request.url);
    if (url.pathname.startsWith('/')) {
      let day = new Date()
      if (day.getDay() % 2) {
        url.hostname = day1
      } else {
        url.hostname = day2
      }
      let new_request = new Request(url, request);
      return fetch(new_request);
    }
    return env.ASSETS.fetch(request);
  },
};
```
</details>

### 客户端配置

```
  - 别名: "yourName"
    协议: vless
    地址: yourName.workers.dev
    端口: 443
    用户id: yourUuid
    流控: xtls-rprx-direct
    加密: none
    伪装域名: yourName.workers.dev
    传输类型: ws
    路径: /
    传输层安全：tls   跳过证书验证：false
    SNI：yourName.workers.dev
```



特别鸣谢：Misaka、rptec、DaoChen6大佬提供脚本、教程和思路


