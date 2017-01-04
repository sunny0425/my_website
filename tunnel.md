# 内网穿透 - 将域名指向本地开发

##参考
[https://gist.github.com/fnando/1101211](https://gist.github.com/fnando/1101211)

##条件

1. 可远程访问的服务器：如 username@dev.example.com
2. 指向该服务器的域名，如：dev.example.com

##步骤

###服务器上nginx 配置 /etc/nginx/sites-available/tunnel.conf

```
  upstream tunnel {
    server 127.0.0.1:9000; 
  }
  
  server {
    listen 80; 
    server_name dev.example.com;
    
    location / {
      proxy_set_header  X-Real-IP  $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_redirect off;
      
      proxy_pass http://tunnel;
    }
  }
```
###重启 nginx

```
  $ cd /etc/nginx/sites-enabled;
  $ sudo ln -s /etc/nginx/sites-available/tunnel.conf tunnel.conf
  $ sudo nginx -s reload
```

###如果服务器上有防火墙，把本地的外网ip（通过ip138.com获取）添加到防火墙白名单

  例: fail2ban修改 /etc/fai2ban/jail.local的 ignoreip 
  
###将远程服务器的9000端口映射到本地的3000端口

```
ssh -vNR 9000:localhost:3000 deploy@demo.1kuaizhuan.cn
```

###项目相关（微信测试公众号及本地代码）的域名修改
