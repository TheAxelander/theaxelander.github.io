# Configuration

Settings can be defined via Docker environment variables or via `appsettings.json` file which is located in the root folder of the application.

## Database

### Supported databases

OpenBudgeteer requires a connection to a database which can be established using various variables. Currently, the following database servers are supported:

| CONNECTION_PROVIDER | Database system                         |
|---------------------|-----------------------------------------|
| sqlite              | SQLite                                  |
| mysql               | Oracle MySQL, MariaDB (FOSS MySQL fork) |

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

### Additional comments

- The usage of `CONNECTION_ROOT_PASSWORD` is optional in case user and database for MariaDB or MySQL are not existing and should be created by OpenBudgeteer.

### Database setup

For all supported database provider, once the database is running and accessible for OpenBudgeteer the required tables and initial data will be created on first startup. Database schema changes after an update will be also applied automatically. Please consider a database backup before updating OpenBudgeteer.

#### Sqlite

Not much to do here. OpenBudgeteer will create the database file `database/openbudgeteer.db` in its directory automatically.

#### MariaDB / MySQL

There are two ways to setup MariaDB / MySQL database:

Use `CONNECTION_ROOT_PASSWORD` to let OpenBudgeteer create user and database. The variable is only required for the first startup and can be removed later.

Alternative approach is to create user and database on your own. Important is that OpenBudgeteer has access full write access to the database as well as rights for changing the schema (like creating or changing tables).

An easy way to do that would be to use something like `phpmyadmin`. Create a new user and ensure that you activate both options

- Create database with same name and grant all privileges.
- Grant all privileges on wildcard name (username\_%).

## App Settings

| Variable            | Description                                                                                                | Default                 |
|---------------------|------------------------------------------------------------------------------------------------------------|-------------------------|
| APPSETTINGS_CULTURE | Localization identifier to set things like Currency, Date and Number Format. Must be a BCP 47 language tag | en-US                   |
| APPSETTINGS_THEME   | Sets the [Bootswatch](https://bootswatch.com) Theme that will be used.                                     | default                 |
