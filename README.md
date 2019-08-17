# Grafana and InfluxDB

An optimized docker version of Grafana with InfluxDB as data source.  
All persistent volumes are already set and they are mounted at container run.  
You could edit configuration file of all services, a Nginx reverse proxy extend web server functionalities like TLS connections in production environments.  

## Quickstart 

**1 - Environment File**  
Create a new environment variables `.env` file in project root folder and set respective value as shown below:

```
GF_SERVER_ROOT_URL=http://your.domain.com
GF_SECURITY_ADMIN_PASSWORD=mypassword

INFLUXDB_DB=db_name
INFLUXDB_HTTP_AUTH_ENABLED=true
INFLUXDB_ADMIN_USER=db_user
INFLUXDB_ADMIN_PASSWORD=db_passwd

```

**2 - Create configuration files**  
Clone following file w/o `-sample` suffix in file name:  

- grafana/defaults-sample.ini
- influxdb/influxdb-sample.conf

CLI Commands

```
cp grafana/defaults-sample.ini grafana/defaults.ini
cp influxdb/influxdb-sample.conf influxdb/influxdb.conf 
```

Feel free to change configuration files with your custom values.


**3 - Build Docker images**    
Build new images stack in order to configure them with custom parameters.

```
docker-compose build .
```

**4 - Run Docker containers stack**  
Run Docker containers by Docker Compose
```
docker-compose up -d
```

Stop containers
```
docker-compose down
```

**5 - Connect to Grafana UI**  
Run in your browser Grafana URL and login into the UI

```
# Local environment
http://localhost

# Online instance
http://your.domain.com
```

## Containers Stack
The service is composed by three containers:

- **Grafana**: Charts and metrics UI
- **InfluxDB**: Time series database to store all metrics
- **Nginx**: Reverse proxy to manage HTTP/S requests and proxy to Grafana
 

### Grafana

You can apply your custom configuration in `grafana/defaults.ini` file


### InfluxDB

Current version: **1.5.4**

Test connection to InfluxDB

```
# Run a container as InfluxDB client and send a request to curl
GRAFANA_INFLUXDB_NET=`docker network ls | grep grafana-influxdb | tr -s ' ' | cut -d ' ' -f 2`
docker run --rm -t \
    --network=$GRAFANA_INFLUXDB_NET \
    influxdb:1.5.4 \
    curl -G http://influxdb:8086/query -u db_user:db_passwd --data-urlencode "q=SHOW DATABASES"
```

### Nginx
