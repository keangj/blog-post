#  ssh

## 远程执行

执行远程脚本
``` sh
ssh user@ip 'sh test.sh'
```

本地脚本远程执行
``` sh
ssh user@ip 'bash -s' < test.sh
```

