# Zerotier Moon 搭建步骤

## 第一步 在云服务器上安装 zerotier-one

### 方法一 更简单

     curl -s https://install.zerotier.com | sudo bash

### 方法二 更安全

     curl -s 'https://raw.fastgit.org/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \
     > if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi

## 第二步 云服务器加入虚拟网络

     执行命令，将云服务器加入到自己创建好的虚拟网络，将命令中的 xxxxxxxx 替换成实际的虚拟网络 ID

     sudo zerotier-cli join xxxxxxxx
