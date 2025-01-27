# Quick Start

For a quick ramp-up up of OpenBudgeteer using Docker and MariaDB use below docker compose.

```yml
services:
  openbudgeteer:
    image: axelander/openbudgeteer:latest
    #image: axelander/openbudgeteer:pre-release
    #image: axelander/openbudgeteer:1.7
    container_name: openbudgeteer
    ports:
      - 8080:8080
    environment:
      - CONNECTION_PROVIDER=mariadb
      - CONNECTION_SERVER=openbudgeteer-mariadb
      - CONNECTION_PORT=3306
      - CONNECTION_DATABASE=openbudgeteer
      - CONNECTION_USER=openbudgeteer
      - CONNECTION_PASSWORD=openbudgeteer
      - CONNECTION_ROOT_PASSWORD=myRootPassword # only required for initial DB creation
      - APPSETTINGS_CULTURE=en-US
      - APPSETTINGS_THEME=solar
    depends_on:
      - mariadb
      
  mariadb:
    image: mariadb
    container_name: openbudgeteer-mariadb
    environment:
      MYSQL_ROOT_PASSWORD: myRootPassword
    volumes:
      - data:/var/lib/mysql
      
  # optional    
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: openbudgeteer-phpmyadmin
    links:
      - mariadb:db
    ports:
      - 8081:80
        
volumes:
  data:
```
