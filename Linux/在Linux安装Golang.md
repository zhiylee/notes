```shell
#下载
wget https://go.dev/dl/go1.17.8.linux-amd64.tar.gz

#解压
tar -C /usr/local -xzf go1.17.6.linux-amd64.tar.gz

#设置环境变量
echo export PATH=$PATH:/usr/local/go/bin | tee -a /etc/profile #所有用户
echo export PATH=$PATH:~/go/bin | tee -a /etc/profile

echo export PATH=$PATH:/usr/local/go/bin >> ~/.profile #当前用户



#重载环境变量
source ~/.profile

#使用阿里云镜像
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct
```

