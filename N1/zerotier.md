# Zerotier Moon 搭建步骤

## 第一步 在云服务器上安装 zerotier-one并配置Moon

### 方法一 更简单

     curl -s https://install.zerotier.com | sudo bash

### 方法二 更安全

     curl -s 'https://raw.fastgit.org/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \
     if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi

## 第二步 云服务器加入虚拟网络

执行命令，将云服务器加入到自己创建好的虚拟网络，将命令中的 ID 替换成实际的虚拟网络 ID

     sudo zerotier-cli join ID

## 第三步 配置 Moon

进入 zerotier-one 程序所在的目录，默认为 /var/lib/zerotier-one

     cd /var/lib/zerotier-one

生成 moon.json 配置文件

     sudo zerotier-idtool initmoon identity.public >> moon.json

编辑 moon.json 配置文件

     sudo nano moon.json

将配置文件中的 "stableEndpoints": [] 修改成 "stableEndpoints": ["ServerIP/9993"]，将 ServerIP 替换成云服务器的公网IP
（主要是添加公网IP，公网IP是服务器的IP，9993是zerotier的默认端口，你服务器防火墙上需要开放UDP:9993,否则是连接不上Moon的）

在生成的moon.json中增加stableEndpoints配置节点，9993为默认端口号，可以修改。

        "stableEndpoints": ["47.xxx.xxx.xxx/9993"]

        ## 如果要修改端口，需要增加primaryPort配置，同时stableEndpoints中的端口号也要改成对应的。
        
        "primaryPort": "10086"

生成 .moon 文件

     sudo zerotier-idtool genmoon moon.json

将生成的 000000xxxxxxxxxx.moon 移动到 moons.d 目录
     sudo mkdir moons.d
     sudo mv 000000xxxxxxxxxx.moon moons.d

.moon 配置文件的名一般为10个前导零+本机的节点ID

重启 zerotier-one 服务

     sudo systemctl restart zerotier-one

到这里，服务器的moon就配置完成了。
对客户端安装zerotier后，将配置好的moon文件配置到客户端，并重启zerotier完成与moon的连接

## 第二步 客户端配置

### Linux端配置

    使用之前步骤中 moon.json 文件中的 id 值 (10 位的字符串，就是xxxxxx），不知道的话在服务器上执行如下命令可以得到id。

    执行命令：
         grep id /var/lib/zerotier-one/moon.json | head -n 1

    然后在客户端机器里执行命令：

    执行命令：
         zerotier-cli orbit xxxxxxx xxxxxxx

    此处的xxxxxxx刚刚在服务器得到的ID值

### Windows配置

打开服务程序services.msc, 找到服务ZeroTier One, 并且在属性内找到该服务可执行文件路径,并且在其下建立moons.d文件夹,然后将moon服务器下生成的000xxxx.moon文件，拷贝到此文件夹内。再重启该服务即可(计算机右键管理-找到服务双击打开-找到zerotier one右键重新启动即可)

路径一般是Windows: C:\ProgramData\ZeroTier\One

## 常用命令

  启动命令

     sudo systemctl start zerotier-one

  重启命令

     sudo systemctl restart zerotier-one

  设置开机自启命令

     sudo systemctl enable zerotier-one
