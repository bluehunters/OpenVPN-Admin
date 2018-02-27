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
  *openvpn服务器IP：10.8.0.1，VPS本地IP：10.6.1.111，VPS网关:10.6.8.122。 客户端连上vpn后，获取IP：10.8.0.6，可以ping通10.8.0.1、10.6.1.111。但是不能ping通VPS网关:10.6.8.122，所以也就不能上网。 VPS环境：centos5.6，已设置net.ipv4.ip_forward=1... 展开
此问题已解决，找了公司的一个大牛解决。公布下解决方法，就是把iptables的转发规则中的 -o eth0 去掉，即：iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE，然后就可以ping通了。 我用的是北京电信通的VPS，有志同道者的朋友可以按此方法解决。

后来又出现了新问题，可以ping通，但是不能上网，DNS确定设置木有问题，可以ping通8.8.8.8 。我的解决方法：清除所有规则 : iptables -F , iptables -X iptables -Z, 然后iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE, /etc/init.d/iptables save 。一切搞定。
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
