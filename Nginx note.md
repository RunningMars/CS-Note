



### Nginx 反向代理 服务器

Nginx是一个高性能的HTTP和反向代理web服务器

### 命令

```shell
#查看nginx版本号
nginx -v

#启动
nginx

#关闭
nginx -s stop

#重新加载
nginx -s reload

#检测配置文件
nginx -t
```



### 配置文件

/usr/local/nginx/conf/nginx.conf

#### 全局块

配置文件开始到events块之间的内容,主要设置nginx整体运行的配置指令

​	worker_processes 并发处理

#### Events块

events的指令主要影响nginx服务器与用户的网络连接

#### Http块

配置最频繁的部分 

##### 	http全局块

##### 	server块

​     虚拟主机的配置



### 正向代理

用户通过nginx访问互联网

客户端浏览器需要配置代理

<img src="Nginx note.assets/image-20220816133808126.png" alt="image-20220816133808126" style="zoom:40%;" />







### 反向代理

把真实服务隐藏在反向代理服务器后面,对外暴露nginx的端口

<img src="Nginx note.assets/image-20220816134151076.png" alt="image-20220816134151076" style="zoom:30%;" />



```nginx

server {
  
		listen 9001;
  	server_name localhost;
  
    location ~ /edu/ {
          proxy_pass http://hlocalhost:8001;
  	}
  
    location ~ /vod/ {
          proxy_pass http://hlocalhost:8080;
  	}
  
} 

```

1,  =    用于不含正则表达式的uri前,要求请求字符串与uri颜哥匹配 

2,  ~     用于标识uri包含正则表达式,并且区分大小写 

3,  ~*   用于标识uri包含正则表达式,并且不区分大小写

4,  ^~  用于不含正则表达式的 uri前, 要求nginx服务器找到标识uri和请求字符串匹配高度的location后



### 负载均衡

分发策略

1 轮询 (默认)

每个请求按时间顺序逐个分配到不同的后端服务器, 如果后端服务器down掉, 能自动剔除.

2 weight

weight 代表权重,默认为1 , 权重越高分配的越多

3 ip_hash

按每个请求访问的ip的hash结果分配,这样每个方可固定访问一个后端服务器, 可以解决session的问题

4 fair (第三方)

按照后端服务器的响应时间来分配请求, 响应时间越短的优先分配

```nginx
http {
    upstream myserver {
        ip_hash;
        server 192.168.0.112:8080 weight=1;
        server 192.168.0.118:8082 weight=1;
    }
  
    server {
   		  listen 80;
    		server_name localhost;
    
        location / {
						proxy_pass http://myserver;
      			proxy_connect_timeout 10;
        }
    }
}
```





### 动静分离

<img src="Nginx note.assets/image-20220816143715394.png" alt="image-20220816143715394" style="zoom:30%;" />

动态资源和静态资源分开部署,通常用专用的静态资源服务器存放静态资源

由nginx实现动静分离

autoindex on 自动

```nginx
http {
   
		server {
        listen 80;
        server_name localhost;

        location /www/ {
             root /data/;
             index index.html index.htm;
        }
    
        location /image/ {
             root /data/;
             autoindex on;
        }
		}
}
```


### 高可用

...



### nginx原理

一个master进程,管理若干个worker进程,work进程争抢client请求

每个worker都是一个独立进程, worker 最佳数量是和cpu核数相同

作为http反向代理的最大并发数量应该是 worker_connectons * worker_processes / 4



































