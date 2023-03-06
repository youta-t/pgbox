pgbox
=======

Start Postgres Sandbox with Docker

Install
---------

```
cd /INSTALL/PATH
git clone https://github.com/youta-t/pgbox.git
echo PATH="${PATH}:$(pwd)/pgbox/pgbox" >> ~/.bashrc  # or somewhere
```

Usage
-------

```
pgbox - start postgres sandbox as container

Usage:

    pgbox [flags] [--] [PATH]

Arguemnts:

  PATH      Path to a file or directory where contains DDLs.
            This is used as '/docker-entrypoint-initdb.d' (or its content) in the container.
            If missing, you will get empty database.

Flags:

  -d, --dbname PGDATABASE       database name.
                                (default: envvar PGDATABASE, or 'postgres')
  -U, --username PGUSER         username of database.
                                (default: envvar PGUSER, or 'postgres')
  -P, --password PASSWORD       password of database.
                                (default: generate randomly)
  -p, --port PGPORT             port to expose.
                                (default: envvar PGPORT, or 5432)
  --no-daemon                   start container as non-daemon.
  -I, --image     IMAGE         image name. (default: postgres)
  -V, --image-version VERSION   image version. (default: latest)
  --name                        container name.
                                (default: pgbox-{RANDOM-SUFFIX})
  -h, --help, help              show this message.

Environment Variables:

  PGDATABASE, PGUSER, PGPORT    default value of --dbname, --username and --port
  DOCKER                        path to docker command (or compatible).
                                pgbox depends on behaviours "docker run" and "docker container inspect".

```

### Example

```
$ cat sandbox/ddl.sql 
create table "example" (
    "id" serial not null,
    "value" text,
    primary key ("id")
);

insert into "example" ("value") values
    ('hello world'),
    ('こんにちわ世界')
;
$ pgbox ./sandbox/
📛 Your container is named 'pgbox-jg27'
🎁 strating... 
Unable to find image 'postgres:latest' locally
latest: Pulling from library/postgres
Digest: sha256:50a96a21f2992518c2cb4601467cf27c7ac852542d8913c1872fe45cd6449947
Status: Downloaded newer image for postgres:latest
🚢 Container seems started!

Your postgres may be accessible by... (if docker is compatible with docker)

  docker exec -it pgbox-jg27 psql

$ docker exec -it pgbox-jg27 psql
psql (15.2 (Debian 15.2-1.pgdg110+1))
Type "help" for help.

postgres=# select * from "example";
 id |     value      
----+----------------
  1 | hello world
  2 | こんにちわ世界
(2 rows)

postgres=# 
\q
$ docker rm $(docker stop pgbox-jg27)
pgbox-jg27
$
```

Dependency
-----------

- bash
- docker (or something compatible)
    - When you use non-docker command, set environment variable `DOCKER` .
    - podman or nerdctl may be avaiable (not tested).

License
-------

MIT

Created by
-----------

youta-t ( youta.t@gmail.com )
