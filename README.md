# Manjaro安装记录  -  By Taozi

## 	1.安装完成Manjaro，初始配置镜像源

### 		1.打开终端，输入命令：

```
$ sudo pacman-mirrors -i -c China -m rank
```

​				这时系统会对中国的镜像进行测速，并在弹出窗口中列出,刷新缓存：

```
$ sudo pacman -Syy
```

### 		2.添加一下ArchLinuxCN的源,打开终端，输入命令：

```
$ sudo gedit /etc/pacman.conf	# Gnome中
$ sudo nano /etc/pacman.conf	# KDE中
```

​				在打开的文件的末尾添加如下内容：

	[archlinuxcn]
	SigLevel = Optional TrustedOnly
	Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
	注:可以通过文件夹打开手动添加
​				注：可以直接进入根目录下的etc目录找到pacman.conf文件，直接修改

### 		3.更新缓存以及导入密钥链：

```
$ sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

​				**注意**，你在执行更新命令时可能会报错，这时你只需再执行一遍即可。

```
$ sudo pacman -Syy	# 这一条命令是用来刷新更新缓存的。
```

### 		4.更新系统：

```
$ sudo pacman -Syyu
```

​	最后重启系统，Manajro的配置与更新基本完成。

```
$ reboot
```



## 	2.解决由于系统本身引起的一些问题

### 		1.VMware中Manjaro无法复制黏贴（文件）的问题：

​				首先卸载open-vm-tools:

```
$ sudo pacman -R open-vm-tools
```

​				下载VMware Tools补丁：

```
git clone https://github.com/rasa/vmware-tools-patches.git
```

​				进入vmware-tools-patches目录：

```
$ cd vmware-tools-patches
```

​				运行补丁：

```
$ sudo ./patched-open-vm-tools.sh
```

​				重启：

```
$ reboot
```

### 		2.Manjaro修改主目录为英文

​				代码如下：

```
$ sudo pacman -S xdg-user-dirs-gtk
$ export LANG=en_US
$ xdg-user-dirs-gtk-update #会有个窗口提示语言更改，更新名称即可
$ export LANG=zh_CN.UTF-8 #然后重启电脑如果提示语言更改，保留旧的名称即可
```

### 		3.解决Manjaro中FireFox为英文的问题，并安装 Flash Player

​				安装FireFox浏览器：

```
$ sudo pacman -S --noconfirm firefox
```

​				安装中文插件：

```
$ sudo pacman -S --noconfirm firefox-i18n-zh-cn
```

​				完整命令：

```
$ sudo pacman -S --noconfirm firefox firefox-i18n-zh-cn
```

​				安装 Flash Player

```
yay -S freshplayerplugin
下载 Flash Player 的压缩包: https://get.adobe.com/flashplayer/?loc=cn
在当前目录解压文件，并在终端执行:
$ sudo cp libflashplayer.so /usr/lib/mozilla/plugins
```



### 4.解决Linux 与 Windows 双系统时间不一致的问题

​				在Manjaro中：

```
$ sudo timedatectl set-local-rtc true

$ sudo pacman -S openntpd
$ systemctl restart openntpd
$ systemctl enable openntpd
```

​				在Windows中：

```
# 以管理员身份使用运行
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f

# 以上方法无效或64位系统：
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f
```

### 5.解决KDE安装Wine软件的错误

```
$ yay -S xsettingsd
# 安装gnome-settings-daemon
$ yay -S gnome-settings-daemon
# 设置gnome-settings-daemon自启动
$ sudo cp /etc/xdg/autostart/org.gnome.SettingsDaemon.XSettings.desktop ~/.config/autostart

$ sudo pacman -S wine-mono
$ sudo pacman -S wine-gecko

# Linux下更改QQ，TIM,微信的DPI
env WINEPREFIX="$HOME/.deepinwine/Deepin-QQ" /usr/bin/deepin-wine winecfg
env WINEPREFIX="$HOME/.deepinwine/Deepin-TIM" /usr/bin/deepin-wine winecfg
env WINEPREFIX="$HOME/.deepinwine/Deepin-WeChat" /usr/bin/deepin-wine winecfg

# WINE-QQ 头像图片加载异常时，执行以下命令 
$ sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
$ sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
$ sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1

# 微信失去焦点出现透明边框时，先点击置顶，再取消即可
# 当微信不能发图片
# 进入 https://github.com/countstarlight/deepin-wine-wechat-arch/releases 下载兼容版本微信
$ sudo pacman -U deepin-wine-wechat-2.7.1.88-1-x86_64.pkg.tar.xz
```



## 	3.安装软件

### 		1.中文输入法（谷歌拼音）

​				安装输入法：

```
$ sudo pacman -S fcitx-im  # 默认全部安装
$ sudo pacman -S fcitx-configtool
$ sudo pacman -S fcitx-googlepinyin
```

​				添加输入法配置文件：

```
$ sudo gedit ~/.xprofile	# Gnome中
$ sudo nano ~/.xprofile 	# KDE中
```

​				在文件中添加如下内容：

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
注:可以带当前用户主目录下手动创建 ".xprofile" 文件并写入
```

​				重启系统：

```
$ reboot
```

### 		2.Vim

```
$ sudo pacman -S vim
```

### 		3.AUR助手（以防官方仓库没有想要的软件）

```
$ sudo pacman -S yay
```

### 		4.Goolgle Chrome

```
$ sudo pacman -S google-chrome
```

### 		5.Markdown编辑器 Typora

```
$ sudo pacman -S typora
```

### 		6.WPS

```
$ sudo pacman -S wps-office
$ sudo pacman -S ttf-wps-fonts
$ sudo pacman -S wps-office-mui-zh-cn #解决WPS安装后菜单栏为英文的问题
```

### 		7.安装中文字体

```
$ sudo pacman -S ttf-roboto noto-fonts ttf-dejavu
$ sudo pacman -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei  # 文泉驿
$ sudo pacman -S noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts # 思源字体
```

### 		8.网易云音乐

```
$ sudo pacman -S netease-cloud-music
```

### 		9.QQ

```
$ sudo pacman -S linuxqq
$ sudo pacman -S deepin.com.qq.im		#QQ
$ sudo pacman -S deepin.com.qq.office	#Tim
```

### 		10.WeChat

```
$ sudo pacman -S deepin-wine-wechat
```

### 		11. Qv2ray

```
$ sudo pacman -S  qv2ray
# 阅读官方文档：https://qv2ray.github.io/getting-started
# 下载v2ray-code：https://github.com/v2fly/v2ray-core/releases
# 下载Qv2ray：https://github.com/Qv2ray/Qv2ray/releases
# 将v2ray-code放置在~/.config/qv2ray/vcore/目录下
# 添加订阅即可
```

### 		12.neofetch

```
$ sudo pacman -S neofetch
```

### 13.screenfetch

```
$ sudo pacman -S screenfetch
```

### 14.gparted分区管理工具

```
$ sudo pacman -S gparted
```

### 		15.虚拟机

```
virtualbox
$ sudo pacman -S virtualbox

vmware-workstation
$ sudo pacman -S vmware-workstation

VMware Workstation Pro 16许可证密钥：

ZF3R0-FHED2-M80TY-8QYGC-NPKYF

YF390-0HF8P-M81RQ-2DXQE-M2UT6

ZF71R-DMX85-08DQY-8YMNC-PPHV8
```



## 	4.开发工具

### 		1.代码编辑器 VSCode

```
$ sudo pacman -S visual-studio-code-bin
```

### 		2.OPENJDK8

```
安装 openjdk8
$ sudo pacman -S jdk8-openjdk
$ sudo pacman -S jre8-openjdk
$ sudo pacman -S jre8-openjdk-headless

卸载 openjdk8
$ sudo pacman -Rs jdk8-openjdk
$ sudo pacman -Rs jre8-openjdk
$ sudo pacman -Rs jre8-openjdk-headless
```

### 3.MySQL

```
$ sudo pacman -S mysql #下载MySQL

$ sudo mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql #初始化MySQL数据库

$ sudo systemctl enable mysqld.service #设置开机自启

$ sudo systemctl start mysqld.service #启动MySQL服务

$ systemctl status mysqld.service #查看MySQL服务状态

$ mysql -uroot -p #连接数据库输入密码

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; #使用SQL语句修改默认密码

# 帮助文档
[client]
# 设置 mysql 客户端 socket 文件位置
socket = /usr/local/share/mysql/mysql.sock

[mysql]
# 设置mysql客户端默认字符集
default-character-set = UTF8MB4
socket = /usr/local/share/mysql/mysql.sock

[mysqld]
innodb_buffer_pool_size = 128M

# mysql 安装目录
basedir = /usr/local/share/mysql

# mysql 数据存储目录
datadir = /usr/local/share/mysql/data

# 端口
port = 3306

# 本机序号为1，表示master
server_id = 1

# 设置 mysql 服务端 socket 文件位置
socket = /usr/local/share/mysql/mysql.sock

# 设置mysql最大连接数
max_connections = 20

# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server = UTF8MB4

# 创建新表时将使用的默认存储引擎
default-storage-engine = INNODB

# 联合查询操作所能使用的缓冲区大小，和sort_buffer_size一样，该参数对应的分配内存也是每连接独享。
join_buffer_size = 128M

# MySQL执行排序使用的缓冲大小。如果想要增加ORDER BY的速度，首先看是否可以让MySQL使用索引而不是额外的排序阶段。
sort_buffer_size = 8M

# MySQL的随机读缓冲区大小。当按任意顺序读取行时(例如，按照排序顺序)，将分配一个随机读缓存区。
read_rnd_buffer_size = 4M 

# 一个事务，在没有提交的时候，产生的日志，记录到Cache中；等到事务提交需要提交的时候，则把日志持久化到磁盘。
# 默认binlog_cache_size大小32K
binlog_cache_size = 1M

# 这个值（默认8）表示可以重新利用保存在缓存中线程的数量，当断开连接时如果缓存中还有空间，那么客户端的线程将被放到缓存中，
# 如果线程重新被请求，那么请求将从缓存中读取,如果缓存中是空的或者是新的请求，那么这个线程将被重新创建,如果有很多新的线程，
# 增加这个值可以改善系统性能.通过比较Connections和Threads_created状态的变量，可以看到这个变量的作用。(–>表示要调整的值)
# 根据物理内存设置规则如下：
# 1G  —> 8
# 2G  —> 16
# 3G  —> 32
# 大于3G  —> 64
thread_cache_size = 8

# MySQL的查询缓冲大小（从4.0.1开始，MySQL提供了查询缓冲机制）使用查询缓冲，MySQL将SELECT语句和查询结果存放在缓冲区中，
# 今后对于同样的SELECT语句（区分大小写），将直接从缓冲区中读取结果。根据MySQL用户手册，使用查询缓冲最多可以达到238%的效率。
# 通过检查状态值'Qcache_%'，可以知道query_cache_size设置是否合理：如果Qcache_lowmem_prunes的值非常大，则表明经常出现缓冲不够的情况，
# 如果Qcache_hits的值也非常大，则表明查询缓冲使用非常频繁，此时需要增加缓冲大小；如果Qcache_hits的值不大，则表明你的查询重复率很低，
# 这种情况下使用查询缓冲反而会影响效率，那么可以考虑不用查询缓冲。此外，在SELECT语句中加入SQL_NO_CACHE可以明确表示不使用查询缓冲
# query_cache_size = 8M

# 指定单个查询能够使用的缓冲区大小，默认1M
# query_cache_limit = 2M

# 指定用于索引的缓冲区大小，增加它可得到更好处理的索引(对所有读和多重写)，到你能负担得起那样多。如果你使它太大，
# 系统将开始换页并且真的变慢了。对于内存在4GB左右的服务器该参数可设置为384M或512M。通过检查状态值Key_read_requests和Key_reads，
# 可以知道key_buffer_size设置是否合理。比例key_reads/key_read_requests应该尽可能的低，
# 至少是1:100，1:1000更好(上述状态值可以使用SHOW STATUS LIKE 'key_read%'获得)。注意：该参数值设置的过大反而会是服务器整体效率降低
key_buffer_size = 4M

# 分词词汇最小长度，默认4
ft_min_word_len = 4

# MySQL支持4种事务隔离级别，他们分别是：
# READ-UNCOMMITTED, READ-COMMITTED, REPEATABLE-READ, SERIALIZABLE.
# 如没有指定，MySQL默认采用的是REPEATABLE-READ，ORACLE默认的是READ-COMMITTED
transaction_isolation = REPEATABLE-READ

# 默认时区
default-time_zone = '+8:00'

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO
```

### 4. Navicat 

```
# Linux下的破解版 MySQL 图形化工具
激活教程: http://navicat.rainss.cc/
下载地址: http://navicat.rainss.cc/down.php
激活工具: http://navicat.rainss.cc/
不能连接MySQL的解决方法: https://blog.csdn.net/swan_tang/article/details/104607100
```



## 5.常用pacman命令

### 		1.更新系统

```
$ sudo pacman -Syu		对整个系统进行更新（常用）	
$ sudo pacman -Syy		强制更新	
$ sudo pacman -Syudd		使用 -dd跳过所有检测
```

### 		2.安装包

```
$ sudo pacman -S 包名		例如，执行 pacman -S firefox 将安装 Firefox。你也可以同时安装多个包，只需以空格分隔包名即可
$ sudo pacman -Sy 包名		与上面命令不同的是，该命令将在同步包数据库后再执行安装
$ sudo pacman -Sv 包名		在显示一些操作信息后执行安装
$ sudo pacman -U	包名		安装本地包，其扩展名为 pkg.tar.gz
$ sudo pacman -U ulr		http://www.example.com/repo/example.pkg.tar.xz 安装一个远程包（不在 pacman 配置的源里面）
```

### 		3.删除包

```
$ sudo pacman -R 包名		该命令将只删除包，保留其全部已经安装的依赖关系
$ sudo pacman -Rs 包名		在删除包的同时，删除其所有没有被其他已安装软件包使用的依赖关系
$ sudo pacman -Rsc 包名	在删除包的同时，删除所有依赖这个软件包的程序
$ sudo pacman -Rd 包名		在删除包时不检查依赖
```

### 		4.搜索包

```
$ sudo pacman -Ss 关键字	在仓库中搜索含关键字的包
$ sudo pacman -Qs 关键字	搜索已安装的包
$ sudo pacman -Qi 包名		查看有关包的详尽信息
$ sudo pacman -Ql 包名		列出该包的文件
```

### 		5.其他用法

```
$ sudo pacman -Sw 包名		只下载包，不安装
$ sudo pacman -Sc		清理未安装的包文件，包文件位于 /var/cache/pacman/pkg/ 目录
$ sudo pacman -Scc		清理所有的缓存文件
```

### 		6.清理垃圾

```
$ sudo pacman -Rsn $(pacman -Qdtq)
$ sudo pacman -Scc
$ sudo rm /var/lib/systemd/coredump/.
$ sudo journalctl --vacuum-size=50M
```

### 7.manjaro无法锁定数据库

```
出现错误的原因是，之前同步的时候，我手动中断了进程，导致之前进程锁文件未被释放。
解决办法，删掉之前的文件（root）：
$ sudo rm /var/lib/pacman/db.lck
```

