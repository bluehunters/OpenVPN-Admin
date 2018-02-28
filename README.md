# OpenVPN Admin

## Summary
Administrate its OpenVPN with a web interface (logs visualisations, users managing...) and a SQL database.

![Previsualisation configuration](https://lutim.cpy.re/fUq2rxqz)
![Previsualisation administration](https://lutim.cpy.re/wwYMkHcM)


## Prerequisite

  * GNU/Linux with Bash and root access
  * Fresh install of OpenVPN
  * Web server (NGinx, Apache...)
  * MySQL
  * PHP >= 5.5 with modules:
    * zip
    * pdo_mysql
  * bower
  * unzip
  * wget
  * sed
  * curl

### Debian Jessie

````
# apt-get install openvpn apache2 php5-mysql mysql-server php5 nodejs unzip git wget sed npm curl
# npm install -g bower
# ln -s /usr/bin/nodejs /usr/bin/node
````

### CentOS 7

````
# yum install epel-release
# yum install openvpn httpd php-mysql mariadb-server php nodejs unzip git wget sed npm
# npm install -g bower
# systemctl enable mariadb
# systemctl start mariadb
````

### Other distribution... (PR welcome)

## Tests

Only tested on Debian Jessie. Feel free to open issues.

## Installation

  * Setup OpenVPN and the web application:

        $ cd ~/my_coding_workspace
        $ git clone https://github.com/Chocobozzz/OpenVPN-Admin openvpn-admin
        $ cd openvpn-admin
        # ./install.sh www_base_dir web_user web_group
     

  * Setup the web server (Apache, NGinx...) to serve the web application.
  * Create the admin of the web application by visiting `http://your-installation/index.php?installation`

## Usage

  * Start OpenVPN on the server (for example `systemctl start openvpn@server`)
  * Connect to the web application as an admin
  * Create an user
  * User get the configurations files via the web application (and put them in */etc/openvpn*)
  * Users on GNU/Linux systems, run `chmod +x /etc/openvpn/update-resolv.sh` as root
  * User run OpenVPN (for example `systemctl start openvpn@client`)
  * goto internet （route add -net 10.8.0.0/24 gw x.x.x.x）,add default route.
  *
 ## 解决路由问题
net.ipv4.ip_forward=1
把iptables的转发规则中的 -o eth0 去掉，即：
iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE
然后就可以ping通了

后来又出现了新问题，可以ping通，但是不能上网，DNS确定设置木有问题，可以ping通8.8.8.8 。我的解决方法：
清除所有规则 : 
Centos 6.5
```
  iptables -F , iptables -X iptables -Z
  iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE, /etc/init.d/iptables save 
```

Centos7
```
 在CentOS 7中，iptables防火墙已经被firewalld所取代，需要使用如下方法：

首先启动firewalld,先查看防火墙状态

systemctl status firewalld.service
service firewalld start

查看有哪些服务已经在列表中允许通过：

# firewall-cmd --list-services
dhcpv6-client http https ssh

可以看到已经有了dhcpv6-client, http, https, ssh四项，接下来添加openvpn：


# firewall-cmd --add-service openvpn 
success
# firewall-cmd --permanent --add-service openvpn 
success

检查一下：

# firewall-cmd --list-services
dhcpv6-client http https openvpn ssh

最后添加masquerade:

# firewall-cmd --add-masquerade 
success
# firewall-cmd --permanent --add-masquerade 
success

以下命令用于确认masquerade是否添加成功：

# firewall-cmd --query-masquerade
yes
```


## Update

    $ git pull origin master
    # ./update.sh www_base_dir

## Desinstall
It will remove all installed components (OpenVPN keys and configurations, the web application, iptables rules...).

    # ./desinstall.sh www_base_dir

## Use of

  * [Bootstrap](https://github.com/twbs/bootstrap)
  * [Bootstrap Table](http://bootstrap-table.wenzhixin.net.cn/)
  * [Bootstrap Datepicker](https://github.com/eternicode/bootstrap-datepicker)
  * [JQuery](https://jquery.com/)
  * [X-editable](https://github.com/vitalets/x-editable)
