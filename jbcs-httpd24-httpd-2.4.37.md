```
JBCS rpm version is available via yum install (yum groupinstall jbcs-httpd24)

yum install httpd
yum install mod_ssl

chmod -R 755 /var/www/html
chown -R apache:apache /var/www/html

sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload

systemctl restart httpd
systemctl stop httpd
systemctl start httpd
systemctl status httpd

systemctl status firewalld.service
systemctl stop firewalld.service

vi /etc/httpd/conf/httpd.conf
cd /var/www/html
tail -n 20 /var/log/httpd/error_log
vi /etc/hosts

ps aux | grep httpd
lsof -i :80
netstat -tuln | grep :80
ss -tuln | grep :80
sudo kill -9 <PID>
```

