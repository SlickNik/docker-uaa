## Running a MySQL/MariaDB container to store data

The sample `dev.yml` configuration tells the UAA server to store its
data in a MySQL database. If you change it to something else,
then skip to the next chapter.

To easily create a MariaDB database on your local dev environment,
use Docker. The following command creates a local
[MariaDB container](https://hub.docker.com/_/mariadb/)
with a default database called 'uaa' and a default user
'root'.

``` docker run -d --name uaa-db mariadb ```

## Running the UAA server

To run the UAA server, first you'll need a configuration file. UAA
accepts a YAML config file where the default clients and users can be
defined among some other things, like where to store the data. You can
find a basic configuration in the project repository. The default
configuration stores data in a postgresql database whose connection
parameters are defined in environment variables (that are
automatically set when a database container is linked with the name
*db*).

This docker container reads the configuration file `uaa.yml` from the
`/uaa` folder. The container can accept configuration files from an
URL, or from a shared volume. To run a UAA server with a configuration
file in a shared volume, run this command:

``` docker run --link uaa-mariadb -p 8080:8080 -e DB_ADDR=uaa-mariadb
-e DB_PORT=3306 -e DB_ENV_DB=uaa -e DB_ENV_USER={user} -e
DB_ENV_PASS={password} -d --name docker-uaa docker-uaa:latest
```

If you are using boot2docker on OSX, host volume sharing only shares
the host folder in boot2docker, so make sure your configuration is in
boot2docker's `/tmp/uaa` folder.

To get the configuration from an URL:

``` docker run -d --link uaa-mariadb -e UAA_CONFIG_URL={URL}
docker-uaa:latest ```
