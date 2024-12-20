# API

To interact with the database via API, you need start a seperate API container like below:

``` yml
services:
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
      - data:/var/lib/mysql

volumes:
  data:
```

The documentation is accessible via Swagger UI using the entpoint `/swagger`.
