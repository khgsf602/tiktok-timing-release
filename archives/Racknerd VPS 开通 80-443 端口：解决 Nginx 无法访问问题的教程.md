# Racknerd VPS 开通 80-443 端口：解决 Nginx 无法访问问题的教程

在使用 Racknerd 购买海外 VPS 后，我选择了 CentOS 7 作为操作系统，但在安装好 Nginx 后发现外网无法正常访问。这篇文章将为大家详细记录如何解决此问题，通常这是因为防火墙策略未开放相关端口所致。

👉 [【建议收藏】2025年Racknerd最新优惠套餐整理汇总 - 每日更新可用活动优惠](https://bit.ly/Rack_Nerd)

## 1. 测试 Nginx 是否安装成功

在开始排查问题之前，我们需要确认 Nginx 是否正确安装并运行。可以通过以下命令进行本地访问测试：

bash
$ wget http://127.0.0.1
--2025-01-03 20:11:34--  http://127.0.0.1/
正在连接 127.0.0.1:80... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：615 [text/html]
正在保存至: “index.html”
2025-01-03 20:11:34 (83.5 MB/s) - 已保存 “index.html” [615/615]


以上输出表明，Nginx 已经正常运行。

## 2. 检查 80/443 端口是否开放

随后需要查看当前防火墙配置，检查 80 和 443 端口是否开放。使用以下命令查询：

bash
$ firewall-cmd --query-port=80/tcp
output: no
$ firewall-cmd --query-port=443/tcp
output: no


如果返回 `no`，说明防火墙未开放这些端口。

## 3. 开放防火墙端口

使用 `firewall-cmd` 命令添加需要开放的端口规则：

bash
$ firewall-cmd --permanent --add-port=80/tcp
output: success
$ firewall-cmd --permanent --add-port=443/tcp
output: success


添加完成后，必须重启防火墙以使配置生效。

## 4. 重启防火墙

重启防火墙的命令如下：

bash
$ firewall-cmd --reload
output: success


经过上述步骤后，外网应该已经能够正常访问 Nginx。

## 防火墙常用指令总结

以下是一些常用的防火墙管理命令，可作为日常运维时的参考。

### 查看防火墙服务运行状态

bash
$ systemctl status firewalld


### 检查防火墙是否正常运行

bash
$ firewall-cmd --state


### 查看当前防火墙规则

bash
$ firewall-cmd --list-all


### 查询特定端口是否开放

bash
$ firewall-cmd --query-port=19999/tcp


### 开放某个端口（如 80 端口）

bash
$ firewall-cmd --permanent --add-port=80/tcp


### 关闭某个端口（如 80 端口）

bash
$ firewall-cmd --permanent --remove-port=80/tcp


### 重启防火墙

bash
$ firewall-cmd --reload


通过以上步骤和命令，您应能顺利解决 Nginx 无法外网访问的问题，并熟练使用防火墙相关工具进行服务器的网络配置管理。