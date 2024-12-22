```
sudo yum -y install /opt/edb2/el_8_x86_64/*.rpm

## install EDB

ls | wc -l
ls | grep edb-as15-server

## install PgBouncer

yum install 
ls | grep edb-pgbounce

pgbouncer -V
psql --version

Check PgBouncer Logs - journalctl -u pgbouncer.service -n 50
 
find /etc/systemd/system/ /lib/systemd/system/ -name '*pgbouncer*'
systemctl list-units --type=service

sudo systemctl start edb-pgbouncer-1.23.service
sudo systemctl status edb-pgbouncer-1.23.service
```