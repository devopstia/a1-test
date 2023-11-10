
## Requirements
- Use datadog to minitor containers
- Added a new DB for mysql

```sh
## Variables for UI service:
AUTH_HOST= auth
AUTH_PORT=8080
WEATHER_HOST= weather
WEATHER_PORT= 5000
REDIS_USER=redis
REDIS_PASSWORD=redis

## Variables for Weather service:
APIKEY = ecbc396f46mshb65cbb1f82cf334p1fcc87jsna5e962a3c542

## Variables for Auth service:
DB_HOST= db
DB_PASSWORD= my-secret-pw

## Variables for db service:
MYSQL_ROOT_PASSWORD= my-secret-pw

## Variables for redis service:
REDIS_USER=redis
REDIS_PASSWORD=redis

## Variables for redis service:
REDIS_USER=redis
REDIS_PASSWORD=redis	
```


## Volume Mount
```
docker volume ls
ls -l /var/lib/docker/volumes
ls -l /var/lib/docker/volumes/weather-app_db-data/_data/
ls -l /var/lib/docker/volumes/weather-app_redis-data/_data/
```

## Issue with restarting when changing Redis and DB password
- Stop compose
- Delete all volume 
- Start compose 
- This is because the password was already persisted
```
docker-compose down
docker volume ls
docker volume rm weather-app_db-data
docker volume rm weather-app_redis-data
```

## Login and check users that sign up already as root
```sh
docker exec -it [DB_CONTAINER_ID] bash
docker exec -it 470c55283eb9 bash
mysql -u root -p

SHOW DATABASES;
USE auth;
SHOW TABLES;
SELECT * FROM users;
```

## Login and check users that sign up already as admin
```sh
docker exec -it [DB_CONTAINER_ID] bash
docker exec -it 470c55283eb9 bash
mysql -u admin -p weatherapp

SHOW DATABASES;
USE weatherapp;
SHOW TABLES; ## This will be empty because we don't have anything store in there. everything is stored in the default table and admin do not have access. Most login as root
USE auth;
SELECT * FROM users;
```

## To grant the MySQL user "admin" access to all databases, you can follow these steps:
```
mysql -u root -p
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY 'GzWNQY8eH4YBkt8HutZj@*J';
FLUSH PRIVILEGES;
exit
```

## Volume mount on host
mkdir -p /data/redis-data/
mkdir -p /data/db-data

chmod -R 777 /data/redis-data
chmod -R 777 /data/db-data


