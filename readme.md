# OpenWRT 分区

每天使用 GitHub Action Workflow 构建。

该项目仅适用于 OpenWRT 路由器。目前它基于2203。

您可能想要这里的[原始项目](http://openwrt-dist.sourceforge.net/)。

## Openwrt 包生成器

**用法**
* 第1步

首先，添加与私钥key-build配对的公钥simonsmh-dist.pub进行构建。
```
wget http://cdn.jsdelivr.net/gh/simonsmh/openwrt-dist@master/simonsmh-dist.pub
opkg-key add simonsmh-dist.pub
```
* 第2步

您可以从路由器上的 distfeeds 获取目标分支。
```
# cat /etc/opkg/distfeeds.conf
src/gz openwrt_core https://downloads.openwrt.org/releases/21.02.3/targets/x86/64/packages
...
```
这意味着x86/64是您的目标，您现在将packages/ x86/64作为分支名称。

* 第3步

在分支列表中搜索您的分支名称，然后将以下行添加到 `/etc/opkg/customfeeds.conf`.
```
src/gz simonsmh http://cdn.jsdelivr.net/gh/simonsmh/openwrt-dist@{{$BRANCH_NAME}}
```
例如，如果您想使用x86_64包并且分支名称为packages/x86/64，则可以在上一步之后使用此行。
```
src/gz simonsmh http://cdn.jsdelivr.net/gh/simonsmh/openwrt-dist@packages/x86/64
```
* 然后安装任何你想要的。

```
opkg update
opkg install ChinaDNS
opkg install luci-app-chinadns
opkg install dns-forwarder
opkg install luci-app-dns-forwarder
opkg install shadowsocks-libev
opkg install luci-app-shadowsocks
opkg install v2ray-plugin
opkg install luci-app-pdnsd
opkg install v2ray-core
opkg install luci-app-v2ray
opkg install luci-app-vlmcsd
...
```
有关更多详细信息，请查看清单。

您也可以在 LuCI 中搜索并安装它们，或者使用 SCP/SFTP 将这些下载的文件上传到您的路由器，然后登录到您的路由器并使用 opkg 安装这些 ipk 文件。

## Openwrt 图像生成器

SDK 完成构建包后，使用 ImageBuilder 构建可配置的图像。图像存储在名为分支的设备中，例如image/generic.x86_64

[安装参考](https://github.com/simonsmh/openwrt-dist/blob/master/.github/workflows/main.yml)

## 自己建造

[在这里检查](https://github.com/simonsmh/openwrt-dist/blob/master/.github/workflows/main.yml)

您需要自己在矩阵中创建一个 fork 和 chage 项目以满足您的需求。如果您需要确保您的包安全，请使用usign重新生成私钥并使 repo 私有。

## 执照

GPLv3
