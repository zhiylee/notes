```shell
apt install samba


vim /etc/samba/sam.conf

[new-share]
comment = my share
path = /smbshare
browsable = yes
writeable=yes


systemctl restart/start/stop smbd


#添加新用户
smbpasswd -a username
#修改密码
smbpasswd username
```



## Failed to add entry for user

这是因为没有加相应的系统账号，所以会提示Failed to add entry for user的错误，只需增加相应的系统账号test就可以了

```shell
groupadd test -g 6000
useradd test -u 6000 -g 6000 -s /sbin/nologin -d /dev/null
```

这时就可以用smbpasswd -a test增加test这个samba账号了!为了增加系统的安全性，所以加的系统账号不要给shell它，也不给它指定目录，到时在/home目录给test账号建个文件夹，该文件夹只有test有读写权限即可!
如:

```shell
mkdir /home/test
chown -R test:test /home/test
```

若不想让另人访问，只让test用户可以访问，只需执行命令:

```shell
chmod u+rwx,g+rwx,o-rwx /home/test
```