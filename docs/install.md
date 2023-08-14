# Install

## Database

OpenBudgeteer requires a connection to a database. See [Config](config.md#database) for more details.

## Docker

You can use the pre-built Docker image from [Docker Hub](https://hub.docker.com/r/axelander/openbudgeteer).

### docker run

To start a container use a `docker run` command like below. Please note that user and database need to be available, otherwise the container will not work. See [Database](#database) for more details.

```bash
docker run -d --name='openbudgeteer' \
    -e 'CONNECTION_PROVIDER'='mysql' \
    -e 'CONNECTION_SERVER'='192.168.178.100' \
    -e 'CONNECTION_PORT'='3306' \
    -e 'CONNECTION_DATABASE'='MyOpenBudgeteerDb' \
    -e 'CONNECTION_USER'='MyOpenBudgeteerUser' \
    -e 'CONNECTION_PASSWORD'='MyOpenBudgeteerPassword' \
    -e 'CONNECTION_MYSQL_ROOT_PASSWORD'='MyRootPassword' \
    -p '6100:80/tcp' \
    'axelander/openbudgeteer:latest'
```

Alternatively you can use a local `Sqlite` database using the below settings:

```bash
docker run -d --name='openbudgeteer' \
    -e 'CONNECTION_PROVIDER'='sqlite' \
    -v '/my/local/path:/app/database'  \
    -p '6100:80/tcp' \
    'axelander/openbudgeteer:latest'
```

If you don't change the Port Mapping you can access the App with Port `80`. Otherwise like above example it can be accessed with Port `6100`

### docker compose (recommended)

Below an example how to deploy OpenBudgeteer together with MySql Server and phpMyAdmin for administration.  Please note that user and database need to be available, otherwise the container will not work. See [Database](#database) for more details.

```yml
version: "3"

networks:
  app-global:
    external: true
  db-internal:


services:
  openbudgeteer:
    image: axelander/openbudgeteer
    container_name: openbudgeteer
    ports:
      - 8081:80
    environment:
      - CONNECTION_PROVIDER=MYSQL
      - CONNECTION_SERVER=openbudgeteer-mysql
      - CONNECTION_PORT=3306
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
      - APPSETTINGS_CULTURE=en-US
      - APPSETTINGS_THEME=solar
    depends_on:
      - mysql
    networks:
      - app-global
      - db-internal

  mysql:
    image: mysql
    container_name: openbudgeteer-mysql
    environment:
      MYSQL_ROOT_PASSWORD: myRootPassword
    volumes:
      - data:/var/lib/mysql
    networks:
      - db-internal

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: openbudgeteer-phpmyadmin
    links:
      - mysql:db
    ports:
      - 8080:80
    networks:
      - app-global
      - db-internal

volumes:
  data:
```

### Docker tags

Beside the default `latest` tag there is also a `pre-release` tag available which includes the latest developments. Please note that `pre-release` can contain bugs on web frontend and also on database side.

Like for every update it is recommended to make a backup of the database before pulling new docker images.

In case you want to stick to a specific version there are also tags for each release available, like `1.4`, `1.5`, `1.5.1` etc.

## Build and deploy

If you don't want to use Docker you can also build the project on your own and deploy it on a web server like nginx.

Install .NET SDK 6 for your respective Linux distribution. See [here](https://docs.microsoft.com/en-us/dotnet/core/install/linux) for more details. Below example is for Debian 11

```bash
wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-6.0 
```

Install nginx

```bash
sudo apt install nginx

sudo systemctl start nginx 
```

Clone git Repository and Build project

```bash
git clone https://github.com/TheAxelander/OpenBudgeteer.git
cd OpenBudgeteer/OpenBudgeteer.Blazor

dotnet publish -c Release --self-contained -r linux-x64
```

Modify `appsettings.json` and enter credentials for a running database server, or use sqlite

```bash
cd bin/Release/net6.0/linux-x64/publish

nano appsettings.json
```

For MySQL:

```json
{
  "CONNECTION_PROVIDER": "mysql",
  "CONNECTION_DATABASE": "openbudgeteer",
  "CONNECTION_SERVER": "192.168.178.100",
  "CONNECTION_PORT": "3306",
  "CONNECTION_USER": "openbudgeteer",
  "CONNECTION_PASSWORD": "openbudgeteer",
  "CONNECTION_ROOT_PASSWORD": "myRootPassword",
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}

For Sqlite:

```json
{
  "CONNECTION_PROVIDER": "sqlite", 
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

Start server running on port 5000

```bash
./OpenBudgeteer --urls http://0.0.0.0:5000
```
