
History Comandos
```
systemctl enable pacemaker pcsd
systemctl enable corosync
setsebool daemons_enable_cluster_mode=1
pcs constraint location asterisk prefers issabel1_g6=50
pcs constraint location mysql prefers issabel1_g6=50
pcs constraint location drbdlinks prefers issabel1_g6=50
pcs constraint location webserver_fs prefers issabel1_g6=50
pcs constraint location webserver_data_sync prefers issabel1_g6=50
pcs constraint order drbdlinks then asterisk
pcs constraint order webserver_fs then mysql
pcs constraint order webserver_fs then drbdlinks
pcs constraint order webserver_fs then webserver
pcs constraint order promote webserver_data_sync then start webserver_fs
pcs constraint colocation add webserver_fs webserver_data_sync INFINITY with-rsc-role=Master
pcs constraint colocation add webserver mysql INFINITY
pcs constraint colocation add webserver asterisk INFINITY
pcs constraint colocation add webserver drbdlinks INFINITY
pcs constraint colocation add webserver webserver_fs INFINITY
pcs resource create asterisk ocf:heartbeat:asterisk user="root" group="root" op monitor timeout="30"
pcs resource create asterisk ocf:heartbeat:asterisk params user="root" group="root" op monitor timeout="30"
pcs resource create mysql ocf:heartbeat:mysql op monitor interval=10s
pcs resource update drbdlinks ocf:heartbeat:drbdlinks op monitor interval=20s
pcs resource --help
pcs --help
pcs resource create drbdlinks ocf:heartbeat:drbdlinks op monitor interval=10s
chmod +x /usr/lib/ocf/resource.d/heartbeat/asterisk
wget https://raw.githubusercontent.com/ClusterLabs/resource-agents/master/heartbeat/asterisk -O /usr/lib/ocf/resource.d/heartbeat/asterisk
wget https://raw.github.usercontent.com/ClusterLabs/resource-agents/master/heartbeat/asterisk -O /usr/lib/ocf/resource.d/heartbeat/asterisk
micro /etc/drbdlinks.conf
chmod +x /usr/lib/ocf/resource.d/heartbeat/drbdlinks
pcs resource agents ocf:heartbeat | grep drbdlinks
cp -vr drbdlinks /usr/lib/ocf/resource.d/heartbeat/
cd drbdlinks/
git clone https://github.com/linsomniac/drbdlinks.git
tar -zxvf var-www.tgz
tar -zcvf var-www.tgz /var/www/
tar -zxvf var-log-asterisk.tgz
tar -zcvf var-log-asterisk.tgz /var/log/asterisk/
tar -zxvf var-lib-mysql.tgz
tar -zcvf var-lib-mysql.tgz /var/lib/mysql/
tar -zxvf var-spool-asterisk.tgz
tar -zcvf var-spool-asterisk.tgz /var/spool/asterisk/
tar -zxvf usr-lib64-asterisk.tgz
tar -zcvf usr-lib64-asterisk.tgz /usr/lib64/asterisk/
tar -zxvf var-lib-asterisk.tgz
tar -zxcf var-lib-asterisk.tgz
tar -zcvf var-lib-asterisk.tgz /var/lib/asterisk/
tar -zxvf etc-asterisk.tgz
tar -zcvf etc-asterisk.tgz /etc/asterisk
cat /var/www/html/admin/libraries/db_connect.php
amportal chown
cd /datos/
pcs resource create webserver_fs Filesystem device="/dev/drbd0" directory="/datos" fstype="xfs"
pcs resource master webserver_data_sync webserver_data master-max=1 master-node-max=1 clone-max=2 clone-node-max=1 notify=true
pcs resource create webserver_data ocf:linbit:drbd drbd_resource=drbd0 op monitor interval=10s
pcs resource defaults
pcs resource defaults resource-stickiness=100
pcs constraint location webserver prefers issabel1_g6=50
pcs constraint location virtual_ip prefers issabel1_g6=50
pcs constraint order virtual_ip then webserver
pcs constraint colocation add webserver virtual_ip INFINITY
ip a | grep inet
pcs resource create webserver ocf:heartbeat:apache configfile=/etc/httpd/conf/httpd.conf statusurl="http://localhost/server-status" op monitor interval=20s
ping -c1 192.168.18.40
pcs resource create virtual_ip ocf:heartbeat:IPaddr2 ip=192.168.18.40 cidr_netmask=24 nic=eth0:1 op monitor interval=10s
pcs property set no-quorum-policy=ignore
crm_verify -L -V
pcs property set stonith-enabled=false
pcs status corosync
corosync-cfgtool -s
pcs cluster start --all
pcs cluster setup --name asterisk_drbd issabel1_g6 issabel2_g6
pcs cluster auth issabel1_g6 issabel2_g6 -u hacluster -p Duoc.2023
systemctl status pcsd
systemctl start pcsd
echo "Duoc.2023" | passwd --stdin hacluster
echo "Gabo"
systemctl disable httpd asterisk dahdi issabel-portknock issabel-updaterd hylafax iaxmodem mariadb
systemctl stop httpd asterisk dahdi issabel-portknock issabel-updaterd hylafax iaxmodem mariadb
systemctl stop httpd asterisk
systemctl stop httpd
mkdir /datos
mkfs.xfs /dev/drbd0
cat /proc/drbd
drbdadm primary --force drbd0
lsblk
micro /etc/drbd.d/drbd0.res
drbdadm status drbd0
drbdadm up drbd0
drbdadm create-md drbd0
uname -n
micro /etc/drbd.d/global_common.conf
drbdadm create-md drbd0.res
micro
mv micro /bin/
curl https://getmic.ro | bash
nano /etc/drbd.d/global_common.conf
dd if=/dev/zero bs=1M count=1 of=/dev/sdb1; sync
mkfs.xfs /dev/sdb1
cfdisk -z /dev/sdb
cfdisk /dev/sdb
echo "Listen 192.168.18.40:80" | tee --append /etc/httpd/conf/httpd.conf
ping 192.168.18.40
yum install tealdeer
tldr
sed -i "s/RewriteRule/#RewriteRule/" /etc/httpd/conf.d/issabel.conf
sed -i "s/Listen/#Listen/" /etc/httpd/conf/httpd.conf
nano /etc/httpd/conf.d/serverstatus.conf
yum install micro
yum install -y drbd84-utils kmod-drbd84 drbdlinks
uname -msr
package-cleanup --oldkernels
reboot
package-cleanup --oldkernel
yum install yum-utils
yum --enablerepo=elrepo-kernel install kernel-lt
yum list available --disablerepo='*' --enablerepo=elrepo-kernel
yum install kernel
yum update
rpm -q kernel
uname -r
rpm -Va --nofiles --nodigest
yum install -y drdb84-utils kmod-drdb84 drdblinks
yum install -y corosync pcs pacemaker resource-agents drdb84-utils kmod-drdb84 drdblinks git
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
yum repolist all
nano /etc/yum.repos.d/drbdlink.repo
reset && sleep 2 && echo "ISSABEL 1" && ip a && ping issabel2_g6
sleep 2 && ip a && ping issabel2_g6
sleep 2 && ip a | grep inet && ping issabel2_g6
sleep 2 && ping issabel2_g6
ping issabel2_g6
ping issabel1_g6.local
ping issabel1_g6
nano /etc/hosts
yum -y update
ping www.google.com
ping 8.8.8.8
fdisk -l
hostnamectl set-hostname issabel1_g6.local
nmtui
nano
ping 20.20.20.10
ping 20.20.20.1
poweroff
micro /usr/local/sbin/motd.sh
ssh localhost
nano /usr/local/sbin/motd.sh
micro /etc/motd
cd ~
yum install libc6
whereis libc6
ldd --version
string libstdc++.so.6
whereis libstdc++.so.6
whereis libstdc++.so-6
whereis libstdc
string libstdc++.so | grep GLIBCXX
string libstdc++.so* | grep GLIBCXX
string libstdc++.so.6.0.28 | grep GLIBCXX
string libstdc++.so.6.0.28
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
export LD_LIBRARY_PATH=/usr/local/lib/:/usr/lib/:/usr/local/lib64/:/usr/lib64/:/usr/bin/gcc
which gcc
export LD_LIBRARY_PATH=/usr/local/lib/:/usr/lib/:/usr/local/lib64/:/usr/lib64/
export LD_LIBRATY
whereis gcc
sudo yum install libstdc
yum install glibc-2.5
yum install glibc-2.6
yum install glibc-2.7
yum install glibc
dnf install kitty
dnf update
yum install dnf-0.6.4-2.sdl7.noarch.rpm dnf-conf-0.6.4-2.sdl7.noarch.rpm python-dnf-0.6.4-2.sdl7.noarch.rpm
wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64/python-dnf-0.6.4-2.sdl7.noarch.rpm
wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64/dnf-conf-0.6.4-2.sdl7.noarch.rpm
wget http://springdale.math.ias.edu/data/puias/unsupported/7/x86_64//dnf-0.6.4-2.sdl7.noarch.rpm
yum install dnf
kitty
yum install npm
sudo yum install nodejs
yum erase go
yum --help
./dev.sh build
yum install go
git clone https://github.com/kovidgoyal/kitty.git && cd kitty
rm lf-linux-amd64.tar.gz
fc-cache ~/.local/share/fonts/
mv *.ttf ~/.local/share/fonts/
mkdir ~/.local/share/fonts
unzip Hack.zip
cd Descargas/
lsd -lha
eset
yum install gcc git harfbuzz harfbuzz-devel zlib libpng freetype fontconfig fontconfig-devel ImageMagick
yum install rh-python36
yum install centos-release-scl
date
nano /etc/motd
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum localinstall --nogpgcheck https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-7.noarch.rpm --skip-broken
sudo yum localinstall --nogpgcheck https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-7.noarch.rpm
find . -xtype l -exec rm {} \;
find . -xtype l --exec rm {} \;
find . -xtype l
cd /
lf
mv lf /usr/local/bin/
tar xvf lf-linux-amd64.tar.gz
wget https://github.com/gokcehan/lf/releases/download/r31/lf-linux-amd64.tar.gz
sudo timedatectl
yum install sddm
systemctl list-unit-files -t service
ss -tulpn
lsof -i4 -6
nmtui-connect
nmtui-hostname
yum install lsof
yum install lf
yum clean all
yum upgrade
yum check-update
time
hwclock
systemctl disable iptables
systemctl disable firewalld
iptables -L
iptables -X
iptables -F
systemctl stop firewalld
systemctl stop iptables
systemctl status iptables
iptables
echo "user: admin" && echo "Pass: Duoc.2023"
ping 1.1.1.1
micro /etc/sysconfig/network-scripts/ifcfg-eth0
sudo systemctl restart network
dhclient eth0
hostnamectl set-hostname issabel_grupo_6.local
hostnamectl set-hostname issabelgrupo6.local
micro hostname
cd etc/
cd
du -hs /* | sort -k 2
du
find / -xtype l
find / -xtype l -delete
```

En caso de fallo con el portal web, estos comandos
```
iptables -L
iptables -F
iptables -X
systemctl stop iptables
systemctl disable iptables
systemctl stop firewalld
systemctl disable firewalld
amportal chown
```

