# Jasper Server 

Based on [monitoringartist/jasperserver-xxl](https://hub.docker.com/r/monitoringartist/jasperserver-xxl/)

Dockerized [TIBCO JasperReports Server Community Edition](http://community.jaspersoft.com/download). Quick start:

```
# Start persistent container storage for database
docker run -d -v /var/lib/mysql --name jasperserver-db-storage busybox:latest

# Start database - MariaDB (note: no Zabbix data are included in the Docker image)
docker run \
    --name jasperserver-db \
    -v /etc/localtime:/etc/localtime:ro \
    -v /etc/timezone:/etc/timezone:ro \
    --volumes-from jasperserver-db-storage \
    --env="MARIADB_USER=jasper" \
    --env="MARIADB_PASS=my_password" \
    -d zabbix/zabbix-db-mariadb:latest

# Start JasperServer    
docker run \
    --name jasperserver \
    -p 8080:8080 \
    -v /etc/localtime:/etc/localtime:ro \
    -v /etc/timezone:/etc/timezone:ro \
    --link jasperserver-db:jasper.db \
    --env="JS_DB_HOST=jasper.db" \
    --env="JS_DB_USER=jasper" \
    --env="JS_DB_PASSWORD=my_password" \
    -d monitoringartist/jasperserver-xxl:latest

# Starting and autodeployment can take 5-7 minutes, be patient
# You can watch progres and issues in logs
docker logs -f jasperserver
```

JasperServer will be available on URL `http://<YOUR DOCKER HOST IP>:8080/jasperserver`.
Default credentials `jasperadmin/jasperadmin`.

# Configuration

You can use environment variables to configure JasperServer container:

| Environment variable | Default value | Note |
| -------------------- | ------------- | ----- |
| JS_DB_TYPE | mysql | postgres is not supported atm |
| JS_DB_HOST | jasper.db | |
| JS_DB_PORT | 3306 | |
| JS_DB_USER | jasper | |
| JS_DB_PASSWORD | my_password | |
| JS_Xmx | 512m | |
| JS_MaxPermSize | 256m | |
| JS_CATALINA_OPTS | -XX:+UseBiasedLocking -XX:BiasedLockingStartupDelay=0 -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:+DisableExplicitGC -XX:+CMSIncrementalMode -XX:+CMSIncrementalPacing -XX:+CMSParallelRemarkEnabled -XX:+UseCompressedOops -XX:+UseCMSInitiatingOccupancyOnly | |
| JS_ENABLE_SAVE_TO_HOST_FS | false | This enable the scheduled reports to be saved in the host |


# Integrations

* [docker-compose for dockerized jasperserver-xxl](https://github.com/SmartmaticLabs/jasperserver-xxl/blob/master/docker-compose.yml)

# Author

Based on [monitoringartist/jasperserver-xxl](https://hub.docker.com/r/monitoringartist/jasperserver-xxl/)
