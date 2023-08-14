# Quick Start

For a quick ramp-up up of OpenBudgeteer using Docker and Sqlite use below command or docker compose.

## docker run

```bash
docker run -d --name='openbudgeteer' \
    -e 'CONNECTION_PROVIDER'='sqlite' \
    -v '/my/local/path:/app/database'  \
    -p '6100:80/tcp' \
    'axelander/openbudgeteer'
```

## docker compose

```yml
version: "3"

services:
  openbudgeteer:
    image: axelander/openbudgeteer
    container_name: openbudgeteer
    environment:
      - CONNECTION_PROVIDER=sqlite
    volumes:
      - data:/app/database
        
volumes:
  data:
```
