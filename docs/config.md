# Configuration

Settings can be defined via Docker environment variables or via `appsettings.json` file which is located in the root folder of the application.

## Database

### Supported databases

OpenBudgeteer requires a connection to a database which can be established using various variables. Currently, the following database servers are supported:

| CONNECTION_PROVIDER | Database system                         |
|---------------------|-----------------------------------------|
| sqlite              | SQLite                                  |
| mysql               | Oracle MySQL, MariaDB (FOSS MySQL fork) |

!!! warning "Pre-release notice"

    PostgreSQL and Sqlite temp file support is currently only available in `pre-release` and is planned to be released with Update `1.7`.
    Below are the future planned settings for `CONNECTION_PROVIDER`:

    | CONNECTION_PROVIDER | Database system                                      |
    |---------------------|------------------------------------------------------|
    | TEMPDB              | SQLite in a temp file                                |
    | SQLITE              | SQLite. Use CONNECTION_DATABASE to specify file name |
    | MYSQL               | Oracle MySQL                                         |
    | MARIADB             | MariaDB (FOSS MySQL fork)                            |
    | POSTGRES            | PostgreSQL                                           |
    | POSTGRESQL          | PostgreSQL                                           |

Automated database initialization is only supported for MySQL, SQLite and MariaDB.

### Database variables

| Variable                       | Description                          | Example                 |
|--------------------------------|--------------------------------------|-------------------------|
| CONNECTION_PROVIDER            | Type of database that should be used | mysql                   |
| CONNECTION_SERVER              | IP Address to MySQL Server           | 192.168.178.100         |
| CONNECTION_PORT                | Port to MySQL Server                 | 3306                    |
| CONNECTION_DATABASE            | Database name                        | MyOpenBudgeteerDb       |
| CONNECTION_USER                | Database user                        | MyOpenBudgeteerUser     |
| CONNECTION_PASSWORD            | Database password                    | MyOpenBudgeteerPassword |
| CONNECTION_MYSQL_ROOT_PASSWORD | Root Password                        | MyRootPassword          |

!!! warning "Pre-release notice"

    Below details apply for `pre-release` and are planned to be released with Update `1.7`.

    | Variable                 | Description                                             | Used for database provider            | Example                 |
    |--------------------------|---------------------------------------------------------|---------------------------------------|-------------------------|
    | CONNECTION_PROVIDER      | Type of database that should be used                    | All (mandatory)                       | MYSQL                   |
    | CONNECTION_SERVER        | IP Address/FQDN of the database Server                  | MySQL, MariaDB, PostgreSQL (optional) | 192.168.178.100         |
    | CONNECTION_PORT          | Port to database Server                                 | MySQL, MariaDB, PostgreSQL (optional) | 3306                    |
    | CONNECTION_DATABASE      | Database name, for SQLite full path and database name   | All (optional)                        | MyOpenBudgeteerDb       |
    | CONNECTION_USER          | Database user                                           | MySQL, MariaDB, PostgreSQL (optional) | MyOpenBudgeteerUser     |
    | CONNECTION_PASSWORD      | Database password                                       | MySQL, MariaDB, PostgreSQL (optional) | MyOpenBudgeteerPassword |
    | CONNECTION_ROOT_PASSWORD | Root Password                                           | MySQL, MariaDB (optional)             | MyRootPassword          |

### Default values for variables

!!! warning "Pre-release notice"

    Below details apply for `pre-release` and are planned to be released with Update `1.7`.

    | Variable                 | SQLite                         | MySQL, MariaDB | PostgreSQL |
    |--------------------------|--------------------------------|----------------|------------|
    | CONNECTION_PROVIDER      |                                |                |            |
    | CONNECTION_SERVER        |                                | localhost      | localhost  |
    | CONNECTION_PORT          |                                | 3306           | 5432       |
    | CONNECTION_DATABASE      | /app/database/openbudgeteer.db | openbudgeteer  | postgres   |
    | CONNECTION_USER          |                                | openbudgeteer  | postgres   |
    | CONNECTION_PASSWORD      |                                |                |            |
    | CONNECTION_ROOT_PASSWORD |                                |                |            |

### Additional comments

- The usage of `CONNECTION_ROOT_PASSWORD` is optional in case user and database for MariaDB or MySQL are not existing and should be created by OpenBudgeteer.

!!! warning "Pre-release notice"

    Below details only apply for `pre-release` and are planned to be released with Update `1.7`.

    - `CONNECTION_PROVIDER` is case-insensitive, so you can use for example `mysql` or `MYSQL`
    - Using MySQL, MariaDB or PostgreSQL parameter `CONNECTION_DATABASE` can have maximum lenght of 64 chars using below character sets:
        - `0-9`
        - `a-z`
        - `A-Z`
        - `$`, `_`
        - `-` (not for PostgreSQL)

### Database setup

For all supported database provider, once the database is running and accessible for OpenBudgeteer the required tables and initial data will be created on first startup. Database schema changes after an update will be also applied automatically. Please consider a database backup before updating OpenBudgeteer.

#### Sqlite

Not much to do here. OpenBudgeteer will create the database file `database/openbudgeteer.db` in its directory automatically.

!!! warning "Pre-release notice"

    Below details only apply for `pre-release` and are planned to be released with Update `1.7`.

    Not much to do here. Set the path to the database file via `CONNECTION_DATABASE`, everything else will be done by OpenBudgeteer.

#### MariaDB / MySQL

There are two ways to setup MariaDB / MySQL database:

Use `CONNECTION_ROOT_PASSWORD` to let OpenBudgeteer create user and database. The variable is only required for the first startup and can be removed later.

Alternative approach is to create user and database on your own. Important is that OpenBudgeteer has access full write access to the database as well as rights for changing the schema (like creating or changing tables).

An easy way to do that would be to use something like `phpmyadmin`. Create a new user and ensure that you activate both options

- Create database with same name and grant all privileges.
- Grant all privileges on wildcard name (username\_%).

#### PostgreSQL

!!! warning "Pre-release notice"

    Below details only apply for `pre-release` and are planned to be released with Update `1.7`.

    Please consider the container-per database PostgreSQL pattern and let container init take care of the database creation, or create the role and database yourself. In this case, the database created by you must be empty, the role must exist, and should have CREATE permission for all objects in the public schema of the target database.

## App Settings

| Variable            | Description                                                                                                | Default                 |
|---------------------|------------------------------------------------------------------------------------------------------------|-------------------------|
| APPSETTINGS_CULTURE | Localization identifier to set things like Currency, Date and Number Format. Must be a BCP 47 language tag | en-US                   |
| APPSETTINGS_THEME   | Sets the [Bootswatch](https://bootswatch.com) Theme that will be used.                                     | default                 |
