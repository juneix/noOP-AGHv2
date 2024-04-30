# noOP-AGHv2

本方案使用 Linux 系统一键脚本安装 AdGuard Home➕v2rayA，作为透明代理和 DNS 服务器，替换掉劝退小白、操作繁琐的 OpenWrt 软（旁）路由方案。 

我先说个「暴论」， 70% 的人其实压根不需要 Openwrt 除了魔法上网的大部分功能， 90% 的人都是随大流，根据各种教程安装了 OP，结果就是各种配置繁杂的教程，还有各种易冲突的插件。
经过千辛万苦终于折腾完成后，却发现宽带的 IPv6 公网访问没了，NAT1 网络环境也没了。

本方案操作简单，对设备性能要求很低，除了 AGH➕v2，你依然可以安装 CasaOS 随便折腾，比 Openwrt 的扩展性和易用性也强多了。
- IPv6 正常使用，搭配 Lucky 可以轻松实现远程访问
- NAT1 正常使用，XBox、Switch可正常联机，大部分时候不需要游戏加速器
- XBox 可快速修改下载服务器 IP，基本跑满带宽
![xbox-down-ip](https://github.com/juneix/noOP-AGHv2/assets/81808039/efec34fb-0653-4293-85ac-d266fd04f829)
![xbox-speed](https://github.com/juneix/noOP-AGHv2/assets/81808039/38ffa48c-4201-4593-babe-cb3d1a8eb69b)


使用本方案，你需要准备以下设备：
- 一台 Linux 系统的低功耗设备（最低配置：~~让卖家帮忙~~刷了 Armbian 的 20 块玩客云即可）
- 虚拟机创建 Debian、Alpine 等 Linux 系统也行（支持 Docker 部署，但更推荐使用虚拟机独立 IP）
- 一个千兆网口（对，单网卡就行）
> 另一台电脑远程操作，需安装 ssh 工具（Win、Mac 自带终端就行，但新手推荐更简单的 [Xterminal](https://www.terminal.icu/)）  
> 如果实在没电脑……手机使用 Termius、ServerBox 等 ssh 工具也可以。

## 一、安装工具
使用 Xterminal 登录你的 Linux 设备。输入以下一键脚本命令安装所需工具。

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

后台管理地址，IP:2017，更多使用教程见[v2rayA官方文档](https://v2raya.org)

### 1.3 一键安装 AdGuard Home
**一键脚本**  
`curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v`  
后台管理地址，IP:3000，更多教程见 P3TERX 大佬的[AGH设置教程](https://p3terx.com/archives/use-adguard-home-to-build-dns-to-prevent-pollution-and-remove-ads-2.html)

## 二、搭配使用AGH➕v2
### 1. AGH 初始化设置时，网页端口建议 3000，**DNS 端口必须使用默认 53**。

如果提示 53 端口已绑定`bind: address already in use`，请参考[AGH官方文档](https://adguard-dns.io/kb/zh-CN/adguard-home/faq/#bindinuse)解除占用。
文档暂时没中文版本，请打开浏览器翻译功能，推荐[沉浸式翻译扩展](https://immersivetranslate.com/)

### 2. v2rayA 设置参考
右上角⚙️【设置】  
- 透明代理：大陆白名单或者 GFWlist，开启 IP 转发，开启端口分享
- 实现方式：tproxy
- 分流模式：同上（支持 http 和 socks5 代理）
- 防止 DNS 污染：自定义高级设置
- 其他默认
![v2ray-setting](https://github.com/juneix/noOP-AGHv2/assets/81808039/ffff4967-bc13-4e49-8c47-9b96df553d10)

按需【创建】单个节点，或【导入】订阅链接。  
选中 2-6 个节点后，左上角启动，更多详细教程可以参考油管或谷歌搜索。
