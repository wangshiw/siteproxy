# siteproxy 2.0
Siteproxy 2.0 使用了service worker, 使得代理更加稳定, 可以代理了的网站更多。
同时使用hono替代express，速度提高4倍。 支持cloudflare worker部署。
反向代理, 免翻墙访问youtube/google, 支持github和telegram web登录。
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
- 使用hono替代express，速度提高4倍。 
- 支持cloudflare worker部署。
- 支持密码控制代理，知道密码才能访问代理。
- 不需要客户端的任何配置，访问代理网址即可访问全世界。
- 支持telegram web登录。
- enter siteproxy's address, and go surf on internet without censorship
- no proxy setting from client side is needed. zero configuration from client browser

### 部署到cloudflare_worker
```
1. 假设你的域名已经管理在cloudflare名下, 并设置你的代理网站域名的DNS到任意ip， 比如1.1.1.1
2. git clone本项目，并使用文本编辑器打开build/worker.js
3. 搜索http://localhost:5006字符串，将它替换为你的代理网站域名，比如https://your-proxy-domain.name
   同时搜索user22334455,将其修改为你自己想设置的密码。
4. 创建一个worker，并编辑worker，将上一步编辑过的worker.js拷贝粘贴到worker里面，保存部署。
5. 增加一个worker路由， 将your-proxy-domain.name/* 指向刚刚保存的worker。
5. 现在可以直接访问https://your-proxy-domain.name/user-your-password/, 就可以了。注意这里的域名和密码替换为你自己的域名和密码。
```

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
```
1. Assume your domain is already managed under Cloudflare, and set the DNS of your proxy website's domain to any IP, such as 1.1.1.1.
2. Git clone this project, and use a text editor to open build/worker.js.
3. Search for the string http://localhost:5006 and replace it with your proxy website's domain, for example, https://your-proxy-domain.name. Also, search for user22334455 and change it to the password you want to set.
4. Create a worker, edit the worker, and copy and paste the edited worker.js from the previous step into the worker, then save and deploy.
5. Add a worker route, directing your-proxy-domain.name/* to the worker you just saved.
6. Now you can directly visit https://your-proxy-domain.name/user-your-password/, and it should work. Note to replace the domain and password with your own.
```

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
