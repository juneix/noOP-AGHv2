# vmm-alpine

使用群晖虚拟机部署 Alpine 系统，一键脚本安装 AdGuard Home➕v2rayA。

## 一、安装系统

### 1.1 下载 Alpine 镜像
虚拟机专用镜像仅有 60MB，上传到群晖【VMM - 映像 - ISO文件】  
`https://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-virt-3.19.0-x86_64.iso`
![iShot_2023-12-23_18 37 12](https://github.com/juneix/vmm-alpine/assets/81808039/2a1e8d6f-08a9-4c01-a437-ca40d45d2c3a)

### 1.2 安装 Alpine 系统
参考教程➡️`https://www.ioiox.com/archives/25.html`  
CPU、内存都 1 GB，虚拟硬盘 10 GB，其他默认或根据你的实际情况修改。

**系统初始化配置**
![iShot_2023-12-23_18 36 31](https://github.com/juneix/vmm-alpine/assets/81808039/8d55dc5a-2f01-4b2a-89d5-c453ce82b8e7)
进入网页控制台，输入`root`登录，默认没有密码。  
![iShot_2023-12-23_19 17 13](https://github.com/juneix/vmm-alpine/assets/81808039/4dbbebac-5106-4e2d-9ac0-378e6745de32)
然后输入`setup-alpine`开始初始化，需要会一点基础的英文，基本上一路回车确认就行。搞错了也不用怕，输入`setup-alpine`再来一遍就是了。  

参考教程➡️`https://www.qunniao.net/1408.html`

| 设置选项               | 输入内容        |
| -------------------- | -------------- |
| 键盘布局               | cn（两次）      |
| 主机名、网络            | 回车默认        |
| 时区                  | PRC（大写）     |
| 软件源                | 回车默认（待会改）|
| 其他                  | 回车默认        |

- ⚠️允许 ssh 密码远程登录，方便下面输入一键脚本命令。  
  - `Allow root ssh login?` 输入 **yes**  
- ⚠️安装系统到虚拟硬盘  
  - `Which disk(s) would you like to use?` 输入 **sda**（对应 SYNOLOGY Storage）  
  - `How would you like to use it?` 输入 **sys**（安装系统盘），然后输入 **y** 确认格式化
  - 最后重启一下系统，输入 **reboot**

## 二、安装软件
记住上面的 IP 地址（建议去路由器后台给虚拟机分配一个固定 IP），使用你喜欢的 ssh 工具登录到 Alpine。

**常规更新软件包**  
`apk update && apk upgrade`

- （可选）替换清华镜像源  
`sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories`

- （可选）安装 Synology Guest Tool  
`apk add qemu-guest-agent`

### 2.1 一键安装 AdGuard Home
**原始脚本**  
`curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v`  

**加速脚本**  
`curl -s -S -L https://gh.5nav.eu.org/https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v`  

AdGuardHome 安装完就会自动启动了。

### 2.2 一键安装 v2rayA
**原始脚本**  
`sh -c "$(wget -qO- https://github.com/v2rayA/v2rayA-installer/raw/main/installer.sh)" @ --with-v2ray`  

**加速脚本**  
`sh -c "$(wget -qO- https://gh.5nav.eu.org/https://github.com/v2rayA/v2rayA-installer/raw/main/installer.sh)" @ --with-v2ray`  

**安装 iptables 模块**  
`apk add iptables ip6tables`

**启动 v2rayA 服务**  
`rc-service v2raya start`  

**设置 v2rayA 开机自启动**  
`rc-update add v2raya`
