# vmm-alpine

使用群晖虚拟机部署 Alpine 系统，一键脚本安装 AdGuard Home➕v2rayA。

## 下载 Alpine 镜像
![iShot_2023-12-23_18 37 12](https://github.com/juneix/vmm-alpine/assets/81808039/2a1e8d6f-08a9-4c01-a437-ca40d45d2c3a)
虚拟机专用仅有60MB，上传到群晖【VMM - 映像 - ISO文件】  
`https://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-virt-3.19.0-x86_64.iso`

## 安装 Alpine 系统
参考虚拟机安装 Linux 的通用教程。`https://www.ioiox.com/archives/25.html`

## 系统初始化配置
![iShot_2023-12-23_18 36 31](https://github.com/juneix/vmm-alpine/assets/81808039/8d55dc5a-2f01-4b2a-89d5-c453ce82b8e7)
需要会一点基础的英文，基本上一路回车确认就行了。  
`setup-alpine` 

- ⚠️允许 ssh 密码登录，方便下面输入一键脚本命令。  
`PermitRootLogin` 输入 yes  
- ⚠️安装系统到虚拟硬盘  
  - 选择硬盘，输入 sda  
  - `How would you like to use it? ('sys', 'data', 'crypt', 'lvm' or '?' for help) [?] `输入 sys  

## （可选）替换清华镜像源
`sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories`

## 常规更新
`apk update && apk upgrade`

## 一键安装 AdGuard Home
`curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v`

## 一键安装v2rayA
`sh -c "$(wget -qO- https://gh.5nav.eu.org/github.com/v2rayA/v2rayA-installer/raw/main/installer.sh)" @ --with-v2ray`

## 安装 iptables 模块
`apk add iptables ip6tables`

## （可选）安装 Synology Guest Tool
`apk add qemu-guest-agent`
