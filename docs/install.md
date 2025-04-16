# Install

## Database & Redis

OpenBudgeteer requires a connection to a database and to Redis. See [Database](config.md#database) and [Redis](config.md#redis) for more details.

## Docker

You can use the pre-built Docker image from [Docker Hub](https://hub.docker.com/r/axelander/openbudgeteer).

Below an example how to deploy OpenBudgeteer together with MariaDB Server and phpMyAdmin for administration.  Please note that user and database need to be available, otherwise the container will not work. See [Database](#database) for more details.

``` yml
services:
  openbudgeteer:
    image: axelander/openbudgeteer
    container_name: openbudgeteer
    ports:
      - 8081:8080
    environment:
      - CONNECTION_PROVIDER=mariadb
      - CONNECTION_SERVER=openbudgeteer-mariadb
      - CONNECTION_PORT=3306
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
      - CONNECTION_REDIS_SERVER=openbudgeteer-redis
      - APPSETTINGS_CULTURE=en-US
    depends_on:
      - mariadb
      - redis

  # optional
  openbudgeteer-api:
    image: axelander/openbudgeteer-api
    container_name: openbudgeteer-api
    ports:
      - 8082:8080
    environment:
      - CONNECTION_PROVIDER=mariadb
      - CONNECTION_SERVER=openbudgeteer-mariadb
      - CONNECTION_PORT=3306
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
    depends_on:
      - mariadb  

  mariadb:
    image: mariadb
    container_name: openbudgeteer-mariadb
    environment:
      MYSQL_ROOT_PASSWORD: myRootPassword
    volumes:
      - db-data:/var/lib/mysql

  redis:
    image: redis
    container_name: openbudgeteer-redis
    volumes:
      - redis-data:/data

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: openbudgeteer-phpmyadmin
    links:
      - mariadb:db
    ports:
      - 8080:80

volumes:
  db-data:
  redis-data:
```

Below another example how to deploy OpenBudgeteer together with PostgreSQL Server.
Please note that role and database `openbudgeteer` will be created with full authority on the `db` container on the first initialization of the database.

``` yml
services:
  openbudgeteer:
    image: axelander/openbudgeteer
    container_name: openbudgeteer
    ports:
      - 8081:8080
    environment:
      - CONNECTION_PROVIDER=postgres
      - CONNECTION_SERVER=openbudgeteer-db
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
      - CONNECTION_REDIS_SERVER=openbudgeteer-redis
      - APPSETTINGS_CULTURE=en-US
    depends_on:
      - db
      - redis

  # optional
  openbudgeteer-api:
    image: axelander/openbudgeteer-api
    container_name: openbudgeteer-api
    ports:
      - 8082:8080
    environment:
      - CONNECTION_PROVIDER=postgres
      - CONNECTION_SERVER=openbudgeteer-db
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
    depends_on:
      - db

  db:
    image: postgres:alpine
    container_name: openbudgeteer-db
    environment:
      - POSTGRES_USER=openbudgeteer
      - POSTGRES_PASSWORD=openbudgeteer
      - POSTGRES_DB=openbudgeteer
    volumes:
      - db-data:/var/lib/postgresql/data

  redis:
    image: redis
    container_name: openbudgeteer-redis
    volumes:
      - redis-data:/data

volumes:
  db-data:
  redis-data:
```

### Docker tags

Beside the default `latest` tag there is also a `pre-release` tag available which includes the latest developments. Please note that `pre-release` can contain bugs on web frontend and also on database side.

Like for every update it is recommended to make a backup of the database before pulling new docker images.

In case you want to stick to a specific version there are also tags for each release available, like `1.4`, `1.5`, `1.5.1` etc.

## Build and deploy

If you don't want to use Docker you can also build the project on your own and deploy it on a web server like nginx.

Install .NET SDK 9 for your respective Linux distribution. See [here](https://docs.microsoft.com/en-us/dotnet/core/install/linux) for more details. Below example is for Debian 12

``` bash
wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt update; \
  sudo apt install -y apt-transport-https && \
  sudo apt update && \
  sudo apt install -y dotnet-sdk-9.0 
```

Install Redis

``` bash
sudo apt install redis

sudo systemctl start redis 
```

Install nginx

``` bash
sudo apt install nginx

sudo systemctl start nginx 
```

Clone git Repository and Build project

``` bash
git clone https://github.com/TheAxelander/OpenBudgeteer.git
git checkout master
cd OpenBudgeteer/OpenBudgeteer.Blazor

dotnet publish -c Release --self-contained -r linux-x64
```

Create a `.env` file enter credentials for a running database server

``` bash
cd bin/Release/net9.0/linux-x64/publish

nano .env
```

For MariaDB:

``` ini
CONNECTION_PROVIDER=mariadb
CONNECTION_SERVER=192.168.178.100
CONNECTION_PORT=3306
CONNECTION_DATABASE=openbudgeteer
CONNECTION_USER=openbudgeteer
CONNECTION_PASSWORD=openbudgeteer
CONNECTION_ROOT_PASSWORD=myRootPassword
```

For Postgres:

``` ini
CONNECTION_PROVIDER=postgresql
CONNECTION_SERVER=192.168.178.100
CONNECTION_DATABASE=openbudgeteer
CONNECTION_USER=openbudgeteer
CONNECTION_PASSWORD=openbudgeteer
```

Start server running on port 5000

``` bash
./OpenBudgeteer --urls http://0.0.0.0:5000
```

For API, repeat the steps using `OpenBudgeteer/OpenBudgeteer.API` directory.
