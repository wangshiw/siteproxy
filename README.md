# siteproxy 2.0
Siteproxy 2.0 使用了service worker, 使得代理更加稳定, 可以代理了的网站更多。
反向代理, 免翻墙访问youtube/google, 支持telegram web登录。
pure web page proxy to google/youtube, zero configuration from client side. Reverse proxy to all internet. 一键部署，翻墙利器。

```
                                                 +----> google/youtube
                             +----------------+  |
                             |                |  |
user browser +-------------->+ siteproxy      +-------> wikipedia
                             |                |  |
                             +----------------+  |
                                                 +----> chinese forums
```
请勿将本项目用于非法用途，否则后果自负。

## 目录

- [特点](#特点)
- [部署到cloudflare_worker](#部署到cloudflare_worker)
- [部署到vps或者云服务器](#部署到vps或者云服务器)
- [cloudflare_worker_deployment](#cloudflare_worker_deployment)
- [vps_deployment](#vps_deployment)
- [联系方式](#联系方式)

### 特点
- 支持密码控制代理，知道密码才能访问代理。
- 不需要客户端的任何配置，访问代理网址即可访问全世界。
- 支持telegram web登录。
- enter siteproxy's address, and go surf on internet without censorship
- no proxy setting from client side is needed. zero configuration from client browser

### 部署到cloudflare_worker
注意：siteproxy 2.0尚未支持cloudflare部署。

### 部署到vps或者云服务器
```
1. 创建一个ssl website(使用certbot and nginx, google下用法), 配置nginx,
   /etc/nginx/sites-enabled/default 需要包含以下内容:
   ...
   server {
      server_name your-proxy.domain.name
      location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass       http://localhost:5006;
      }
   }
2. 执行:sudo systecmctl restart nginx
3. 用户环境下执行下列命令安装node环境, 如果你已经有node环境, 忽略这一步
   (1)curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
   (2)source ~/.bashrc
   (3)nvm install v18
4. 执行:git clone https://github.com/netptop/siteproxy.git;
5. 执行:cd siteproxy;
6. 打开并修改保存config.json文件:
   {
      "proxy_url": "https://your-proxy.domain.name", // 这个是你申请到的代理服务器域名
      "token_prefix": "/user-SetYourPasswordHere/",  // 这个实际上是你的网站密码，用来防止非法访问
      "description": "注意:token_prefix相当于网站密码，请谨慎设置。 proxy_url和token_prefix合起来就是访问网址。"
   }
7. 执行:nohup node bundle.js &
8. 现在就可以在浏览器中访问你的域名了, 网址就是前面的proxy_url加上token_prefix.
9. 如果想套CloudFlare加速, 可以参考CloudFlare说明
```
### cloudflare_worker_deployment
Not supported yet for siteproxy2.0

### vps_deployment
```
1. Create an SSL website (using certbot and nginx, Google the usage), configure nginx, /etc/nginx/sites-enabled/default should contain the following content: ... server { server_name your-proxy.domain.name; location / { proxy_set_header X-Real-IP $remote_addr; proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; proxy_set_header X-Forwarded-Proto $scheme; proxy_pass http://localhost:5006; } }
2. Execute: sudo systemctl start nginx
3. Under user environment, run the following commands to install Node environment, if you already have Node environment, skip this step:
    (1) curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
    (2) source ~/.bashrc
    (3) nvm install v18
4. Execute: git clone https://github.com/netptop/siteproxy.git;
5. Execute: cd siteproxy;
6. Open and modify the config.json file and save:
   { 
      "proxy_url": "https://your-proxy.domain.name",
      "token_prefix": "/user-SetYourPasswordHere/",
      "description": "Note: token_prefix acts as the website password, please set it carefully. proxy_url and token_prefix together form the access URL." 
   }
7. Execute: node bundle.js
8. Now you can access your domain in the browser, address is actually proxy_url+token_prefix.
9. If you want to use CloudFlare for acceleration, you can refer to CloudFlare's documentation.
```
### 联系方式
Telegram群: @siteproxy
<br />
email: netptop@gmail.com
