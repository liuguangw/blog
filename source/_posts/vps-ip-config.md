---
title: vps ip 配置记录
date: 2023-10-14 15:34:11
tags: 
  - IP
---

今天发现我的某个 vps 居然额外多赠送了一个IP，所以记录一下怎么给 vps 添加新的 IP 地址。<!--more-->

## 查看网卡配置

```shell
# 详细配置
ifconfig -a
# 简要信息
ifconfig -s
```

## 附加一个新IP

```shell
# ipv4
ifconfig <网卡名称> inet add <ipv4地址>
# 例如
ifconfig eth0 inet add 1.2.3.4

# ipv6
ifconfig <网卡名称> inet6 add <ipv6地址>
```

## 删除地址

```shell
# ipv4
ifconfig <网卡名称> inet del <ipv4地址>
# ipv6
ifconfig <网卡名称> inet6 del <ipv6地址>
```

