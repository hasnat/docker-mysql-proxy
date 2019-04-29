# mysql-proxy

# Usage with docker-compose

without
```
version: '2'

services:
  db:
    image: mysql:8.0.0
    restart: always
    ports:
      - "3307:3306" #for external connection
    volumes:
      - ../mysql-data/db:/var/lib/mysql #mysql-data
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: dbuser
      MYSQL_USER: dbuser
      MYSQL_PASSWORD: password
```

within
```
version: '2'

services:
  mysql:
    image: mysql:8.0.0
    restart: always
    ports:
      - "3307:3306" #for external connection
    volumes:
      - ../mysql-data/db:/var/lib/mysql #mysql-data
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: dbuser
      MYSQL_USER: dbuser
      MYSQL_PASSWORD: password
  db:
    image: bscheshir/mysqlproxy:0.8.5
    ports:
      - "3308:3306" #for external connection
    restart: always
    volumes: 
      - ../mysql-proxy-conf:/opt/mysql-proxy/conf
    environment:
      PROXY_DB_PORT: 3306
      REMOTE_DB_HOST: mysql
      REMOTE_DB_PORT: 3306
    depends_on:
      - mysql
```

# Query to stdout
Query to stdout is default behaviour

# Query logging to file

```
...
    volumes:
      - ../mysql-proxy-conf:/opt/mysql-proxy/conf
      - ../mysql-proxy-logs:/opt/mysql-proxy/logs
    environment:
      PROXY_DB_PORT: 3306
      REMOTE_DB_HOST: mysql
      REMOTE_DB_PORT: 3306
      LOG_FILE: "/opt/mysql-proxy/logs/mysql.log"
...
```

# thanks

https://hub.docker.com/r/zwxajh/mysql-proxy
https://hub.docker.com/r/gediminaspuksmys/mysqlproxy/

> note: Log send to file with delay (buffering mechanism). You can restart the container for get the log immediately. 