# Zerotier Moon 搭建步骤

## 第一步 在云服务器上安装 zerotier-one

    ```bash
    curl -s 'https://raw.fastgit.org/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \
    > if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi
    ```
