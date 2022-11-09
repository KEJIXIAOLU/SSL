# 申请免费的 SSL 证书，证书自动更新

## 准备工作
1、一台境外VPS主流系统，例如：Debian/Ubuntu/CentOS

2、下载并安装FinalShell SSH工具

## 申请 SSL 证书
### Acme 脚本申请证书

用 Acme 脚本申请证书，是我们用到的最常见的一种证书的申请方式，它有很多的申请方法，且不需要实名，大家只需要找到一种适合自己的也就好了。

不管用下面的何种方式申请，都需要安装 Acme，有一部分的申请场景需要用到相关的插件，所以我们需要提前安装。

下面环境的安装方式，大家根据自己的系统选择命令安装就好了。

### 1、Debian/Ubuntu系统执行以下命令：

    apt update -y && apt install -y curl && apt install -y socat
    
 ### 2、CentOS系统执行以下命令：
 
    yum update -y && yum update -y && yum install -y socat
    
 
### 安装 Acme 脚本

    curl https://get.acme.sh | sh
    
 ## 申请证书及密钥
 
 ### 具体操作步骤如下：

1、安装 Acme 脚本之后，请先执行下面的命令（下面的邮箱为你的邮箱）
 
    ~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com

2、根据申请方式选择验证申请的指令。将代码中 <code> mydomain.com </code> 改为你的域名 

 ### 80 端口的验证申请
 如果你还没有运行任何 web 服务, 80 端口是空闲的, 那么 Acme.sh 还能假装自己是一个 WebServer, 临时监听在 80 端口, 完成验证 
 具体操作步骤如下：

    ~/.acme.sh/acme.sh  --issue -d mydomain.com  --standalone
    
  ### 80端口验证申请失败的解决方法：
  
  1、如果是80端口被占用，输入以下指令：放行80端口
  
      iptables -I INPUT -p tcp --dport 80 -j ACCEPT
   
  2、如果是tcp端口已被((‘tnginx，PID=2852，fd=6)，(“nginx”，pid=251，fd=-6)使用，输入以下指令：停止Nginx服务
   
      sudo service nginx stop  
      
      
 ### Nginx 的验证申请
 这种方式需要你的服务器上面已经部署了 Nginx 环境，并且保证你申请的域名已经在 Nginx 进行了 conf 部署。（被申请的域名可以正常被打开）


   ~/.acme.sh/acme.sh --issue  -d mydomain.com   --nginx
   
   
### Http 的验证申请

这种方式需要你的服务器上面已经部署了网站环境。（被申请的域名可以正常被打开）


     ~/.acme.sh/acme.sh  --issue  -d mydomain.com -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/
     
 
 
 ### DNS 验证申请证书 
 
 先将域名托管到 Cloudflare（WS的配置可以开启CDN）
 
 安装系统和面板，再申请证书
 
 1、获取 Cloudflare API 令牌
 在域名管理首页（右下角），获取 API 密钥
 
 ### 安装 ACME
 
    curl https://get.acme.sh | sh
    
 ### 设置 Cloudflare API 令牌
 
    export CF_Key="4f9794c701b6e27884f0da0bab6454de07552"
  
------------ 
 
    export CF_Email="bozai@v2rayssr.com"
    
 ### 验证 DNS 并申请证书
PS：若是DNS验证申请不下来，请尝试使用下面的 Web 验证的方式申请证书   

更改下面的域名为你自己的域名

    ~/.acme.sh/acme.sh --issue --dns dns_cf -d bozai3.xyz -d *.bozai3.xyz

------------

    mkdir /root/cert
    
------------

    ~/.acme.sh/acme.sh --installcert -d bozai3.xyz --key-file /root/cert/private.key --fullchain-file /root/cert/cert.crt

------------

    ~/.acme.sh/acme.sh --upgrade --auto-upgrade
    
------------- 
  
    chmod -R 755 /root/cert
    
    

 ###  安装证书到指定文件夹
 
注意, 默认生成的证书都放在安装目录下: ~/.acme.sh/, 请不要直接使用此目录下的证书文件。
正确的使用方法是使用 <code> --install-cert </code>  命令,并指定目标位置, 然后证书文件会被copy到相应的位置，比如下面的代码
   

    ~/.acme.sh/acme.sh --installcert -d mydomain.com --key-file /root/private.key --fullchain-file /root/cert.crt
    
   
## 更新证书
目前证书在 60 天以后会自动更新, 你无需任何操作. 今后有可能会缩短这个时间, 不过都是自动的, 你不用关心.

### 更新 Acme 脚本
   
       ~/.acme.sh/acme.sh --upgrade


  如果你不想手动升级, 可以开启自动升级:
  
      ~/.acme.sh/acme.sh  --upgrade  --auto-upgrade
 
 
 ## 后记
 
 当然，以上的申请方式并不是全部的 SSL 证书申请方式，只是较为常见而已。关于用 DNS 验证来申请证书的方法，由于篇幅比较长，而且方法也有所区别，我会另外单独做一期搭建教程。感兴趣的朋友可以看下一期的视频。
 
      
    
    
    
