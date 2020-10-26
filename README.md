### 使用 nginx 部署

- [nginx 及其依赖安装](https://www.cnblogs.com/xxoome/p/5866475.html)

> 在安装nginx前首先要确认系统中安装了gcc、pcre-devel、zlib-devel、openssl-devel。
>
> 安装命令：
> ```yum -y install gcc pcre-devel zlib-devel openssl openssl-devel```
>
> nginx下载地址：https://nginx.org/download/

> 下载“nginx-1.9.9.tar.gz”，移动到/usr/local/下。
```
## 解压
tar -zxvf nginx-1.9.9.tar.gz

##进入nginx目录
cd nginx-1.9.9
## 配置
./configure --prefix=/usr/local/nginx

# make
make
make install
```
测试是否安装成功
```
# cd到刚才配置的安装目录/usr/loca/nginx/
./sbin/nginx -t
```

>错误信息：

>nginx: [alert] could not open error log file: open() "/usr/local/nginx/logs/error.log" failed (2: No such file or directory)
2016/09/13 19:08:56 [emerg] 6996#0: open() "/usr/local/nginx/logs/access.log" failed (2: No such file or directory)

>原因分析：nginx/目录下没有logs文件夹

>解决方法：

```
mkdir logs
chmod 700 logs
```
>正常情况的信息输出：
```
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```
>启动nginx 
```
cd /usr/local/nginx/sbin
./nginx //启动nginx
```
>配置nginx开机自启动
```
vim /etc/rc.d/rc.local
```

- 静态文件上传
    将打包好的前端静态文件上传至 nginx 目录下的html文件夹下 /usr/local/nginx/html， 解压 选择 A 覆盖原有文件。

- 配置修改（ngxin.conf）
    - 端口修改
    ```
    listen   ${port}
    ```
    - 代理配置（http请求）
    ```
        location /api {
                proxy_pass http://124.172.188.118:8002/usercenter/api;
        }
        location /passport/signin {
                proxy_pass http://124.172.188.118:8002/usercenter/passport/signin;
        }

        location /ds {
                proxy_pass http://10.57.15.2:8080/ds;
        }
        location /dc {
                proxy_pass http://10.57.15.2:8080/dc;
        }
        location /task {
                proxy_pass http://10.57.15.2:8080/task;
        }
        location /outLineTask {
                proxy_pass http://10.57.15.2:8080/outLineTask;
        }

    ```
    proxy_pass ip 根据实际接口服务器IP进行修改
- 启动nginx 服务
在当前路径下（/usr/local/nginx）
```./sbin/nginx```

- 重启nginx 服务

```./sbin/nginx -s reload```

