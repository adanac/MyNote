##更新sourcelist后并安装yum
```
vi /etc/apt/sources.list

添加 163 的源
deb http://mirrors.163.com/debian wheezy main non-free contrib
deb-src http://mirrors.163.com/debian wheezy main non-free contrib
更新源
root@kali:~/soft# sudo apt-get update

安装yum
sudo apt-get install yum


```