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

!!! warning "Pre-release notice"

    Below details only apply for `pre-release` and are planned to be released with Update `1.7`.

    ```bash
    docker run -d --name='openbudgeteer' \
        -e 'CONNECTION_PROVIDER'='SQLITE' \
        -e 'CONNECTION_DATABASE'='/srv/openbudgeteer.db' \
        -v 'data:/srv'  \
        'axelander/openbudgeteer:pre-release'
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

!!! warning "Pre-release notice"

    Below details only apply for `pre-release` and are planned to be released with Update `1.7`.

    ```yml
    version: "3"
    
    services:
      openbudgeteer:
        image: axelander/openbudgeteer:pre-release
        container_name: openbudgeteer
        environment:
          - CONNECTION_PROVIDER=SQLITE
          - CONNECTION_DATABASE=/srv/openbudgeteer.db
        volumes:
          - data:/srv
            
    volumes:
      data:
    ```
