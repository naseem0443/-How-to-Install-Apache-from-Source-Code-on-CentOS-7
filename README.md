# -How-to-Install-Apache-from-Source-Code-on-CentOS-7


Install Required Packages for Source Code Compilation: <br>

yum groupinstall "Development Tools" -y

yum install expat-devel pcre pcre-devel openssl-devel -y

Download packages for Apache Source Code:

Apache httpd - https://github.com/apache/httpd/releases
Apr - https://github.com/apache/apr/releases
Apr-util - https://github.com/apache/apr-util/releases

Download above mentioned packages either using GUI or below Commands

wget https://github.com/apache/httpd/archive/2.4.46.tar.gz -O httpd-2.4.46.tar.gz

wget https://github.com/apache/apr/archive/1.7.0.tar.gz -O apr-1.7.0.tar.gz

wget https://github.com/apache/apr-util/archive/1.6.1.tar.gz -O apr-util-1.6.1.tar.gz

Extract the Downloaded Packages:

tar -xzvf httpd-2.4.46.tar.gz

tar -xzvf apr-1.7.0.tar.gz

tar -xzvf apr-util-1.6.1.tar.gz

Move the apr and apr-util directory to httpd path:

mv apr-1.7.0 httpd-2.4.46/srclib/apr

mv apr-util-1.6.1 httpd-2.4.46/srclib/apr-util

cd httpd-2.4.46

Start the Compilation Process:

./buildconf

./configure --enable-ssl --enable-so --with-mpm=event --with-included-apr --prefix=/usr/local/apache2

make

make install


Fix httpd Command:

vim /etc/profile.d/httpd.sh

pathmunge /usr/local/apache2/bin

Configure httpd Service Daemon:

vim /etc/systemd/system/httpd.service

---------------------------------------------------------------------------------------------------------------------------
[Unit]
Description=The Apache HTTP Server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/apache2/bin/apachectl -k start
ExecReload=/usr/local/apache2/bin/apachectl -k graceful
ExecStop=/usr/local/apache2/bin/apachectl -k graceful-stop
PIDFile=/usr/local/apache2/logs/httpd.pid
PrivateTmp=true

[Install]
WantedBy=multi-user.target
---------------------------------------------------------------------------------------------------------------------------

systemctl daemon-reload

Start and enable httpd Service:

systemctl start httpd

systemctl enable httpd

Configure the Firewall to open httpd port:

firewall-cmd --permanent --add-service=http
firewall-cmd --reload
firewall-cmd --list-all
