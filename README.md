# noOP-AGHv2

Linux 系统使用一键脚本安装 AdGuard Home➕v2rayA 作为透明代理和 DNS 服务器，替换掉劝退小白、操作繁琐的 OpenWrt 软（旁）路由方案。
先说个「暴论」， 70% 的人其实压根不需要 Openwrt 除了魔法上网的大部分功能，除了各种配置繁杂的教程，还有各种易冲突的插件。
千辛万苦终于折腾完成后，发现宽带的 IPv6 公网没了，NAT1 网络环境也没了。

使用本方案，你需要准备以下设备
- 一台 Linux 系统的低功耗设备（最低配置：20 块玩客云即可）
- 虚拟机创建 Debian、Alpine 等 Linux 系统也行（支持 Docker 部署，但更推荐使用虚拟机独立 IP）
- 千兆网卡
- 另一台电脑远程操作，需安装 ssh 工具（系统自带终端就行，但新手推荐 [Xterminal](https://www.terminal.icu/)）

## 一、安装工具
使用 Xterminal 登录你的 Linux 设备。输入以下 一键脚本命令安装所需工具，

### 1.1 Github520（可跳过）
请确保你的网络可以顺利访问 Github，提供一个[Github520](https://github.com/521xueweihan/GitHub520)项目供参考，如果还不行请自己解决。
`sudo sh -c 'sed -i "/# GitHub520 Host Start/Q" /etc/hosts && curl https://raw.hellogithub.com/hosts >> /etc/hosts'`

### 1.2 一键安装 v2rayA
**一键脚本**  
安装来源是作者的官网，部分地区可能速度较慢，请耐心等待。
`sh -c "$(wget -qO- https://github.com/v2rayA/v2rayA-installer/raw/main/installer.sh)" @ --with-v2ray`  

**安装 iptables 模块**（已安装可跳过）
`apk add iptables ip6tables`

**启动 v2rayA 服务**  
`rc-service v2raya start`  

**设置 v2rayA 开机自启动**  
`rc-update add v2raya`  

后台管理地址，IP:2017，更多使用教程见[官方文档](https://v2raya.org)

### 1.3 一键安装 AdGuard Home
**一键脚本**  
`curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v`  
AdGuardHome 安装完就会自动启动了，后台管理地址，IP:3000  
