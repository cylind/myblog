---
title: Debian10上安装LEMP建站教程
tags: [建站, Linux]
date: 2020-10-01 17:57:10
---

记一次建站过程。宝塔建站是不可能的，lnmp一键脚本又得编译，小机器顶不住哇，所以选择了手动安装Nginx、MariaDB以及PHP的二进制文件来建站，记录如下。

<!-- more -->

下面的过程中，我们使用有sudo权限的用户进行操作。建站前先更新软件源，sudo apt update。

## 安装三大组件

下面开始安装建站中最常见的三大组件Nginx，MariaDB，PHP，它们分别起提供web服务，存储数据和处理内容的作用。

### 安装Nginx

安装web服务器，Nginx。

```sh
sudo apt install nginx
```

安装完后，可在浏览器直接输入ip地址访问，查看效果

### 安装MariaDB

安装sql数据库，MariaDB。MariaDB兼容MySQL，是MySQL的开源替代。

```sh
sudo apt install mariadb-server
```

安装完后进行安全检查：

```sh
sudo mysql_secure_installation
```

首先，它会提示你输入数据库的root密码（注意，不是linux系统root密码），因为是刚刚安装还没设置有密码，所以直接回车即可。

紧接着，它会提示你要不要设置数据库root密码，选择N，因为MariaDB使用的是一种特别的验证方式，通过判断当前linux用户的权限来验证，比设置密码的方式更为安全。

接下来的几项，选Y，最后按回车接受所有默认的子选项即可。这个操作会移除匿名用户和、test database以及禁止root远程登录，并加载刚才你设置的更改规则。

安装完成后，测试一下数据库。登录数据库服务器 MariaDB Server：

```sh
sudo mariadb
```
连接成功后会看到类似如下的输出：
>Welcome to the MariaDB monitor.  Commands end with ; or \g.
>Your MariaDB connection id is 74
>Server version: 10.3.15-MariaDB-1 Debian 10
>
>Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
>
>Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
>
>MariaDB [(none)]> 

### 安装PHP

先是安装基本的php插件php-fpm 和php-mysql，其中php-fpm是负责处理由Nginx传来的PHP请求，而php-mysql则负责与mysql类数据库进行交互。在安装php-fpm、php-mysql时，php会作为依赖安装，所以不必再加入php。

```sh
sudo apt install php-fpm php-mysql
```

然后，告诉PHP只接受服务器上实际存在的文件的URI。 这减轻了一个安全漏洞，即如果文件系统中不存在请求的.php文件，则PHP解释器可能会被诱使允许执行任意代码。
```sh
sudo sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php/7.4/fpm/php.ini
```

## 配置数据库

首先，连接到数据库：

```sh
sudo mariadb
```

然后，开始创建一个名为userdb的数据库

```sql
CREATE DATABASE userdb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

接着，把这个数据的权限授予一个用户管理，这里设置用户名wordpress_user,对应密码设置为password

```sql
GRANT ALL ON userdb.* TO 'user'@'localhost' IDENTIFIED BY 'password';
```

最后，刷新权限

```sql
FLUSH PRIVILEGES;
```

退出MariaDB：

```sql
EXIT;
```

## 安装Typecho

### 下载Typecho

到官网http://typecho.org/download下载最新版，解压并放到/var/www/your_domain文件夹，并更改所有者：

```bash
chown -R www-data:www-data /var/www/your_domain
```



## 安装php插件

不同的模板要求不同的PHP插件，具体看文档要求

```bash
sudo apt install php-curl php-mbstring php-xmlrpc
```

### 配置Nginx

建议直接到https://www.digitalocean.com/community/tools/nginx, 在线生配置文件(建议选有wordpress优化的配置)并按提示覆盖到/etc/nginx目录下。

然后，测试配置文件是否有误：

```sh
sudo nginx -t
```

最后，重载Nginx配置文件使之生效：

```sh
sudo systemctl reload nginx
```

### 登录Typecho

直接登录网站，进行Typecho在线安装配置，过程中需要输入在上面配置的数据库信息。

## 安装Wordpress


### 安装额外PHP插件

不同的模板要求不同的PHP插件，具体看文档要求，Wordpress要安装如下的PHP插件：

```sh
sudo apt install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
```

安装完后，重启php-fpm使上述插件生效：

```sh
sudo systemctl restart php7.4-fpm.service
```

注意，php-fpm有不同的版本，我这里是7.4，所以写的是php7.4-fpm.service，若是版本不同自行更改版本号。可以通过如下命令确定php的版本：

```sh
sudo systemctl status php* | grep fpm.service
```


### 下载WordPress

下载最新版WordPress并复制到/var/www/your_domain下：

```sh
cd /tmp
curl -LO https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
```

在复制全部文件到/var/www/your_domain前，先从wordpress给出示例配置文件中复制一份配置文件模板:

```sh
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```

最后复制全部文件(保留文件权限属性)到/var/www/your_domain：

```sh
sudo cp -a /tmp/wordpress/. /var/www/your_domain
sudo chown -R www-data:www-data /var/www/your_domain
```

### 配置Nginx

建议直接到https://www.digitalocean.com/community/tools/nginx, 在线生配置文件(建议选有wordpress优化的配置)并按提示覆盖到/etc/nginx目录下。

然后，测试配置文件是否有误：

```sh
sudo nginx -t
```

最后，重载Nginx配置文件使之生效：

```sh
sudo systemctl reload nginx
```

### 配置WordPress

首先，从wordpress秘钥生成接口处获取wordpress安全值：

```sh
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

输出内容大概如下：

>```
>define('AUTH_KEY',         '1jl/vqfs<XhdXoAPz9 DO NOT COPY THESE VALUES c_j{iwqD^<+c9.k<J@4H');
>define('SECURE_AUTH_KEY',  'E2N-h2]Dcvp+aS/p7X DO NOT COPY THESE VALUES {Ka(f;rv?Pxf})CgLi-3');
>define('LOGGED_IN_KEY',    'W(50,{W^,OPB%PB<JF DO NOT COPY THESE VALUES 2;y&,2m%3]R6DUth[;88');
>define('NONCE_KEY',        'll,4UC)7ua+8<!4VM+ DO NOT COPY THESE VALUES #`DXF+[$atzM7 o^-C7g');
>define('AUTH_SALT',        'koMrurzOA+|L_lG}kf DO NOT COPY THESE VALUES  07VC*Lj*lD&?3w!BT#-');
>define('SECURE_AUTH_SALT', 'p32*p,]z%LZ+pAu:VY DO NOT COPY THESE VALUES C-?y+K0DK_+F|0h{!_xY');
>define('LOGGED_IN_SALT',   'i^/G2W7!-1H2OQ+t$3 DO NOT COPY THESE VALUES t6**bRVFSD[Hi])-qS`|');
>define('NONCE_SALT',       'Q6]U:K?j4L%Z]}h^q7 DO NOT COPY THESE VALUES 1% ^qUswWgn+6&xqHN&%');
>```

注意，每个人获取到的内容都不同。

然后，开始配置wordpress的配置文件：

```sh
nano /var/www/your_domain/wp-config.php
```

找到如下内容并用刚才执行curl获取到的内容替换：

>```
>define('AUTH_KEY',         'put your unique phrase here');
>define('SECURE_AUTH_KEY',  'put your unique phrase here');
>define('LOGGED_IN_KEY',    'put your unique phrase here');
>define('NONCE_KEY',        'put your unique phrase here');
>define('AUTH_SALT',        'put your unique phrase here');
>define('SECURE_AUTH_SALT', 'put your unique phrase here');
>define('LOGGED_IN_SALT',   'put your unique phrase here');
>define('NONCE_SALT',       'put your unique phrase here');
>```

继续查找，修改数据名称，密码：

```php
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpress_user');

/** MySQL database password */
define('DB_PASSWORD', 'password');
```

此外，还要在这个文件加上两行说明，指定wordpress对文件系统操作方式，禁用连接脚本（不加这个参数的话，使用在digitalocean生成wordpress的nginx配置文件，会使得wordpress后台无法操作），这两行参数可加在任意处。

```php
define('FS_METHOD', 'direct');
define( 'CONCATENATE_SCRIPTS', false );
```

最后，保存并退出。

### 登录WordPress

到这里，wordpress基本部署完毕，剩下的就是到wordpress的登录页面进行初始化设置了。直接到http://server_domain_or_IP处按提示操作即可。

参考：

https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mariadb-php-lemp-stack-on-debian-10

https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-nginx-mariadb-and-php-on-debian-10

https://www.linode.com/docs/web-servers/lemp/how-to-install-the-lemp-stack-on-debian-10/

https://www.tecmint.com/install-lemp-on-debian-10-server/

https://www.hostloc.com/thread-750277-1-1.html

https://gist.github.com/moneal/9859180