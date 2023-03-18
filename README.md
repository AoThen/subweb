# DIY

## 修改位置

sub后端末尾不带/

- `public\conf\config.js`
- `src\views\home`

## 修改后部署

```
npm install
npm run build
```

把生成的dist上传www目录

### nginx 配置
```
server {
    listen 80;
    server_name example.com;

    root /var/www/http/sub-web/dist;
    index index.html index.htm;

    error_page 404 /index.html;

    gzip on; #开启gzip压缩
    gzip_min_length 1k; #设置对数据启用压缩的最少字节数
    gzip_buffers 4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 6; #设置数据的压缩等级,等级为1-9，压缩比从小到大
    gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml; #设置需要压缩的数据格式
    gzip_vary on;

    location ~* \.(css|js|png|jpg|jpeg|gif|gz|svg|mp4|ogg|ogv|webm|htc|xml|woff)$ {
        access_log off;
        add_header Cache-Control "public,max-age=30*24*3600";
    }
}
```



# subweb
## 原文简介
subweb 是基于 subconverter 订阅转换的前端项目,方便用户快速生成各平台的订阅链接.

> *subweb 是我个人入门 vuejs 学习时简单做的一个案例,使用还算方便,开源出来,欢迎各路大佬贡献维护.*

*GitHub [stilleshan/subweb](https://github.com/stilleshan/subweb)  
Docker [stilleshan/subweb](https://hub.docker.com/r/stilleshan/subweb)*
> *docker image support for X86 and ARM*

## 示例
[https://sub.ops.ci](https://sub.ops.ci)  
[https://subweb-demo.vercel.app/](https://subweb-demo.vercel.app/)  
*`前后端示例,可以直接使用.`*

## 部署
### docker 本地版
*适用于本机快速部署使用*
```shell
docker run -d --name subweb --restart always \
  -p 18080:80 \
  stilleshan/subweb
```

访问 `http://127.0.0.1:18080`

### docker 自定义版 + 短链接版
自定义版可以挂载配置文件来修改`API 地址`,`短链接地址`,`站点名称`,`导航链接`.  
参考以下命令,修改本地挂载路径,启动容器后会生成`config.js`配置文件,更改后刷新生效.

```shell
docker run -d --name subweb --restart always \
  -p 18080:80 \
  -v /PATH/subweb/public/conf:/usr/share/nginx/html/conf \
  stilleshan/subweb
```

同时也可以不挂载目录,直接通过`-e`环境变量来修改`API 地址`,`短链接地址`和`站点名称`,但是无法修改`导航链接`.  
`注意:以下域名请严格填写 http 或 https 协议,结尾不要 / 斜杠符号.`
```shell
docker run -d --name subweb --restart always \
  -p 18080:80 \
  -e SITE_NAME=subweb \
  -e API_URL=https://sub.ops.ci \
  -e SHORT_URL=https://s.ops.ci \
  stilleshan/subweb
```

访问 `http://127.0.0.1:18080`  
> *推荐使用 nginx 反向代理部署*

### Vercel 快速部署
为方便 docker 用户方便挂载或环境变量部署,默认`main`分支不支持`Vercel`,需要`fork`仓库少许设置才能部署至`Vercel`.
1. fork 本仓库, 注意**取消**勾选`Copy the main branch only`
2. Vercel 中创建项目使用`subweb`首次部署.
3. 部署完毕后,在`Vercel`项目 - Settings - Git - Production Branch 设置为`vercel`.
4. 修改环境变量 - Settings - Environment Variables 中修改环境变量,具体参考仓库中`.env`文件.
5. Vercel 部署如需修改导航栏信息,需自行修改`src/layouts/components/header/navBarItem.js`文件.

### subweb + subconverter + myurls 合并进阶版
详情查看 [stilleshan/sub](https://github.com/stilleshan/dockerfiles/tree/main/sub)

## 链接
- [stilleshan/sub](https://github.com/stilleshan/dockerfiles/tree/main/sub)
- [stilleshan/subweb](https://github.com/stilleshan/subweb)
- [stilleshan/subconverter](https://github.com/stilleshan/subconverter)
