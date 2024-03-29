
* [ohmyzsh](#ohmyzsh)
* [pip](#pip)
* [git](#git)
* [docker](#docker)
* [gitlab-docker](https://github.com/formateddd/docker-gitlab)
* [shadowsocks](#shadowsocks)



## ohmyzsh

```sh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

themes, 可更改
wget https://raw.githubusercontent.com/formateddd/questions/master/environment/Schminitz.zsh-theme -P ~/.oh-my-zsh/custom/themes/

plgins,
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions

or run it,
sh -c "$(wget https://raw.githubusercontent.com/formateddd/questions/master/environment/zshrc_install.sh -O -)"

```

## ssh/config

```sh
Host virtual
    HostName xxx
    User xxx
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 120
# ssh-copy-id -i ~/.ssh/id_rsa.pub virtual
# ssh -D 1082 -f -C -q -N xx
```

------------
## git

##### public key
``` shell
git config --global user.name ""
git config --global user.email ""
ssh-keygen -t rsa -C ""
```

##### git config
``` shell
# ~/.gitconfig
[http]
proxy = socks5://127.0.0.1:1080
[https]
proxy = socks5://127.0.0.1:1080
```



## pip

```sh
sudo pip install virtualenv
virtualenv -p /usr/bin/python3.7 ~/.virtualenv/py3
# pip freeze > requirements.txt
```

##### pip.conf
```sh
# 直接用国内源安装
pip install web.py -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

##### 修改配置文件：
```sh
# linux:`~/.pip/pip.conf`
# windows:`%HOMEPATH%\pip\pip.ini）`
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

##### 其他源：
```sh
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban) https://pypi.douban.com/simple/
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

##### error: locale.Error: unsupported locale setting
```sh
export LC_ALL=C
```

## docker
#### install
```sh
wget -qO- https://get.docker.com/ | sh
```

#### config path: /etc/docker/daemon.json

```sh
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

```sh
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/.config/jesseduffield/lazydocker lazyteam/lazydocker
```


## shadowsocks
##### server
```sh
pip install shadowsocks

# path : /etc/shadowsocks.json
{
    "server":"0.0.0.0",
    "port_password": {
        "8000": "password"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":true,
    "workers": 1
}

(sudo) ssserver -c /etc/shadowsocks.json -d start
sudo ssserver -d stop

https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E
```


##### client

```sh
pip install shadowsocks

path : ~/.shadowsocks/shadowsocks.json

{
  "server":"my_server_ip",
  "server_port":my_server_port,
  "password":"my_password",
  "local_address": "127.0.0.1",
  "local_port":1080,
  "timeout":300,
  "method":"aes-256-cfb",
  "fast_open":true,
  "pid-file": "/path",
  "log-file": "/path"
}

sslocal -c ~/.shadowsocks.json -d start
可先在系统设置里设置全局代理，在浏览器里安装 https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif
```


##### 开启bbr
```sh
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh

sysctl net.ipv4.tcp_available_congestion_control
#返回值一般为：
#net.ipv4.tcp_available_congestion_control = bbr cubic reno

sysctl net.ipv4.tcp_congestion_control
#返回值一般为：
#net.ipv4.tcp_congestion_control = bbr

sysctl net.core.default_qdisc
#返回值一般为：
#net.core.default_qdisc = fq

lsmod | grep bbr
#返回值有 tcp_bbr 模块即说明bbr已启动。
```

