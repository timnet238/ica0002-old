Backup Restore
Install and configure infrastructure with ansible:

ansible-playbook infra.yaml

MySQL
To restore MySQL data from the backup, run the following commands on the source mysql host (not on the replica):

sudo -u backup duplicity --no-encryption restore rsync://timnet238@backup.clickit.io//home/timnet238/mysql/ /home/backup/restore/
sudo su
mysql agama < /home/backup/restore/agama.sql
To verify:

Query the MySQL database manually or check the web interface of Agama.

InfluxDB
Restore InfluxDB data from the backup:

sudo -u backup duplicity --no-encryption restore rsync://timnet238@backup.clickit.io//home/timnet238/influxdb/ /home/backup/restore/
sudo su
systemctl stop telegraf
influx -execute 'DROP DATABASE telegraf'
influxd restore -portable -database telegraf /home/backup/restore
systemctl start telegraf
To verify:

Query the database manually or check grafana.
