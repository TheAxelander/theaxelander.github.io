# Quick Start

For a quick ramp-up up of OpenBudgeteer using Docker and Sqlite use below command or docker compose.

## docker run

```bash
docker run -d --name='openbudgeteer' \
    -e 'CONNECTION_PROVIDER'='SQLITE' \
    -e 'CONNECTION_DATABASE'='/srv/openbudgeteer.db' \
    -p 8080:8080 \
    -v 'data:/srv'  \
    'axelander/openbudgeteer:latest' # alternatively use 'pre-release' or a specific version tag
```

## docker compose

```yml
version: "3"

services:
  openbudgeteer:
    image: axelander/openbudgeteer:latest
    #image: axelander/openbudgeteer:pre-release
    #image: axelander/openbudgeteer:1.7
    container_name: openbudgeteer
    ports:
      - 8080:8080
    environment:
      - CONNECTION_PROVIDER=SQLITE
      - CONNECTION_DATABASE=/srv/openbudgeteer.db
    volumes:
      - data:/srv
        
volumes:
  data:
```
