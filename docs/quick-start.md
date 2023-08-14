# Quick Start

For a quick ramp-up up of OpenBudgeteer using Docker and Sqlite use below command or docker compose.

## docker run

```bash
docker run -d --name='openbudgeteer' \
    -e 'CONNECTION_PROVIDER'='SQLITE' \
    -e 'CONNECTION_DATABASE'='/srv/openbudgeteer.db' \
    -v 'data:/srv'  \
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
      - CONNECTION_PROVIDER=SQLITE
      - CONNECTION_DATABASE=/srv/openbudgeteer.db
    volumes:
      - data:/srv
        
volumes:
  data:
```
