#

```
docker network create tig
docker rm -f influxdb telegraf

docker run -d --name influxdb -p 8086:8086 --network tig influxdb:2.4.0
docker run -d --name telegraf -p 57000:57000 --network tig telegraf:1.24

```