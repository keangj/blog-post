#  SSH

## 远程执行

执行远程脚本
``` sh
ssh user@ip 'sh test.sh'
```

本地脚本远程执行
``` sh
ssh user@ip 'bash -s' < test.sh
```

## ssh 登录

`ls -al ~/.ssh` 以查看是否存在现有 SSH 密钥

`ssh-keygen`  生成密钥

`-b`参数指定密钥的二进制位数。这个参数值越大，密钥就越不容易破解，但是加密解密的计算开销也会加大。

一般来说，`-b`至少应该是`1024`，更安全一些可以设为`2048`或者更高。

`-C`参数可以为密钥文件指定新的注释，格式为`username@host`

-t 指定密钥的加密算法，如 dsa rsa ed25519

`-f`参数指定生成的私钥文件

`-F`参数检查某个主机名是否在`known_hosts`文件里面

`-N`参数用于指定私钥的密码（passphrase）

`-p`参数用于重新指定私钥的密码（passphrase）。它与`-N`的不同之处在于，新密码不在命令中指定，而是执行后再输入。ssh 先要求输入旧密码，然后要求输入两遍新密码

`-R`参数将指定的主机公钥指纹移出`known_hosts`文件

复制 SSH 公钥

```shell
clip < ~/.ssh/id_ed25519.pub
```

或者找到 `.ssh` 文件夹里的 `.pub` 文件 

### 使用命令行登录服务器

``` sh
ssh -i ~/.ssh/id_ed25519.pub <用户名>@<服务器 ip> -p 23
```

- `-i` 指定私钥
- `-p` 指定端口

配置 `config`

``` 
# 服务器名
Host <服务器名>
    HostName <服务器 IP>
    User <用户名>
    IdentityFile <密钥位置>
```

配置后可使用 `ssh <服务器名>` 登录

