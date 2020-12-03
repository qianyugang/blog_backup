---
title: 启动一个服务器的前五分钟：保障Linux服务器的基本安全性
toc: true
comments: true
categories: 与代码
tags: 
	- Linux

date: 2020-09-08
---

服务器安全不需要很复杂。我的安全理念很简单：采用能够保护免受最常见攻击的原则，同时保持足够高的管理效率，从而不会出现「安全漏洞」。如果你能明智地利用你在服务器上的前5分钟，我相信你能做到。

任何经验丰富的系统管理员都可以告诉您，随着您的发展和增加更多的服务器和开发人员，用户管理不可避免地成为一种负担。在一个快速发展的初创公司环境中，维持传统的访问授权是一场艰苦的战斗——你肯定会得到陈旧的密码、被抛弃的实习账户，以及无数『我有访问服务器a的sudo权限，但没有访问服务器B」的问题。有一些帐户同步工具可以帮助减轻这种麻烦，但以我之见，递增的好处不值得花时间，也不值得在安全性上带来负面影响。简单性是良好安全性的核心。

我们的服务器配置了两个帐户:根帐户和部署帐户。通过任意长的密码，部署用户可以访问sudo，这是开发人员登录的帐户。开发人员使用他们的公钥(而不是密码)登录，因此管理就像在服务器之间保持authorized_keys文件是最新的一样简单。禁用了通过ssh的根登录，并且部署用户只能从我们的office IP来进行登录。

我们的方法的缺点是，如果authorized_keys文件被损坏或被错误授权，我需要登录到远程终端来修复它(Linode提供了一种叫做Lish的东西，它可以在浏览器中运行)。如果您采取适当的谨慎措施，则无需这样做。

注意：我不主张将其作为最安全的方法，而只是为了平衡我们的小型团队的安全性和管理的简便性。根据我的经验，大多数安全漏洞是由安全程序不足或维护不力所导致的。

## 让我们开始吧

我喜欢Ubuntu;如果您使用其他版本的linux，命令可能会有所不同。五分钟配置开始：

```
password
```

将根密码更改为长而复杂的密码。您不需要记住它，只需将它存储在安全的地方—只有当您失去了通过ssh登录的能力或丢失了您的sudo密码时才需要此密码。

```
apt-get update
apt-get upgrade
```
以上这些让我们有了一个好的开始。

## 安装 Fail2ban

```
apt-get install fail2ban
```

[Fail2ban](http://www.fail2ban.org/wiki/index.php/Main_Page)是一个守护程序，它监视对服务器的登录尝试并阻止可疑活动的发生。开箱即用，配置合理。

现在，让我们设置您的登录用户。除了为“ deploy”（部署）之外，还可以随意命名用户，这只是我们使用的一种约定：

```
useradd deploy
mkdir /home/deploy
mkdir /home/deploy/.ssh
chmod 700 /home/deploy/.ssh
```

## 使用公钥验证

固定密码的时代已经结束了。抛弃这些密码并为用户帐户采用[公钥身份验证](http://en.wikipedia.org/wiki/Public-key_cryptography)，从而提高安全性和易用性。

```
vim /home/deploy/.ssh/authorized_keys
```
将本地计算机上id_rsa.pub的内容以及要访问此服务器的任何其他公共密钥添加到此文件中。

```
chmod 400 /home/deploy/.ssh/authorized_keys
chown deploy:deploy /home/deploy -R
```

## 新用户&启用sudo

现在，用部署用户测试您的新帐户登录到新服务器（保持终端窗口的root登录处于打开状态）。如果成功，请以root用户身份切换回终端，并为登录用户设置sudo密码：

```
passwd deploy
```

设置复杂的密码-您既可以将其存储在安全的地方，或者设置一个让人难忘的密码。用于sudo的密码。

```
visudo
```
注释所有现有的用户/组授权行并添加：

```
root    ALL=(ALL) ALL
deploy  ALL=(ALL) ALL
```

当他们输入正确的密码时，将授予sudo访问权限。

## 锁定SSH

配置ssh以防止密码和根登录，并锁定特定ip的ssh:

```
vim /etc/ssh/sshd_config
```

将这些行添加到文件中，插入您将要连接的ip地址:

```
PermitRootLogin no
PasswordAuthentication no
AllowUsers deploy@(your-ip) deploy@(another-ip-if-any)
```

重启SSH：
```
service ssh restart
```

## 设置防火墙


没有防火墙，安全服务器是不完整的。Ubuntu提供了ufw，这使得防火墙管理更加容易。运行:

```
ufw allow from {your-ip} to any port 22
ufw allow 80
ufw allow 443
ufw enable
```


这将设置一个基本的防火墙，并将服务器配置为接受通过端口80和443进行的通信。您可能希望根据服务器要执行的操作添加更多端口。


## 启用自动安全更新


这些年来，我已经习惯了「易于​​获取」的更新/升级习惯，但是在拥有十几台服务器的情况下，我发现登录频率较低的服务器并没有那么新鲜。特别是对于负载平衡的机器，重要的是它们都必须保持最新。自动化的安全更新使我有些恐惧，但不如未修补的安全漏洞严重。

```
apt-get install unattended-upgrades

vim /etc/apt/apt.conf.d/10periodic
```

更新文件如下：

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";

```

还有一个要编辑的配置文件：

```
vim /etc/apt/apt.conf.d/50unattended-upgrades
```

更新文件，如下所示。您可能应该保持禁用更新并仅坚持使用安全更新：

```
Unattended-Upgrade::Allowed-Origins {
        "Ubuntu lucid-security";
//      "Ubuntu lucid-updates";
};
```

## 安装Logwatch来监听

[Logwatch](http://linux.die.net/man/8/logwatch)是一个守护程序，它监视您的日志并将其通过电子邮件发送给您。这对于跟踪和检测入侵很有用。如果有人要访问您的服务器，则通过电子邮件发送给您的日志将有助于确定发生了什么情况以及何时发生-因为服务器上的日志可能已被破坏。

```
apt-get install logwatch

vim /etc/cron.daily/00logwatch
```

添加如下：

```
/usr/sbin/logwatch --output mail --mailto test@gmail.com --detail high
```

完成！

在短短几分钟内，我们锁定了服务器并设置了一定的安全级别，该级别可以抵御大多数攻击，同时易于维护。归根结底，几乎总是用户错误会导致入侵，因此请确保您长时间保存这些密码并确保安全！

## 更新

[Hacker News](http://news.ycombinator.com/item?id=5316093)上发生了一场精彩的讨论。感谢所有的好主意和有用的建议！随着我们基础架构的发展，我肯定会考虑使用Puppet或Chef-它们听起来像是简化多服务器基础架构管理的好工具。如果您像我们一样使用Linode，上面的操作也可以通过StackScripts完成。

## 参考

- **原文链接：**[My First 5 Minutes On A Server; Or, Essential Security for Linux Servers
](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers)
