# docker-lamp-stack
Containerized LAMP stack, separate lamp into apache-php, mysql, and phpmyadmin (3 containers)

## Prerequisite

### for Linux
```
# install docker.io
$ sudo apt-get update
$ sudo apt-get install docker.io

# install docker-compose
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

## Usage guide

### Run the containers and services
```
# First, clone the repo
$ git clone https://github.com/brayce1996/docker-lamp-stack.git

# then switch to the cloned directory
$ cd docker-lamp-stack

# and use `docker-compose` to build the lamp images
$ sudo docker-compose build

# run the services in background
$ sudo docker-compose up -d
```

### Connect to the services
Then, webserver is on 
```
localhost:80
localhost:443
```

To connect the mysql databases, you can use phpmyadmin on
```
localhost:10080
```
with default account and password:
```
account: root
password: admin
```

### Stop the services
when you want to pause it
```
$ sudo docker-compose pause
```
or stop and remove it
```
$ sudo docker-compose down
```

for more instructions, you can reference [docker-compose document](https://docs.docker.com/compose/reference/overview/), or simply get help by
```
$ docker-compose help
```

### Web files
The files under `www/` will be put on the web server

For example,

the default page you see at `http://localhost` is `index.php` under `www/`

### Database files
After running `docker-compose up -d`,
the directories `mysql/data` will bind to `/var/lib/mysql` in mysql container

So you can copy your db files into `mysql/data`
or import your DB by mysql dump and import

## Directories
After cloning docker-lamp-stack, the default structures is like:
```
In docker-lamp-stack/
.
├── docker-compose.yml
├── php
│   ├── Dockerfile
│   ├── log
│   │   └── php_errors.log
│   └── php.ini
└── www
    └── index.php

3 directories, 5 files

```

After running the `docker-compose up -d`, the structure will become
```
.
├── docker-compose.yml
├── mysql
│   └── data 
│       └── ... (many files)
├── php
│   ├── Dockerfile
│   ├── log
│   │   └── php_errors.log
│   └── php.ini
└── www
    └── index.php
```
As you can see, mysql/data is added.

## Edit the settings
### NOTE
If you edit php/Dockerfile or configure a new Dockerfile,

you should run `docker-compose build` to rebuild and apply the changes to your images/containers.

That is because docker-compose use **cache** to speed up, and it will not apply changes until rebuilding the images.

### Install php modules
Most "default" modules are not loaded/installed in the php container

You can use the `RUN` command in php/Dockerfile to install the modules you need.

php-docker provide `docker-php-ext-install` for installing modules (you will need it)

[php docker document](https://hub.docker.com/_/php)

And don't forget to edit/enable the modules in `php/php.ini`,

This configuration will be copied into the container automatically.

## Troubleshoot

### ERROR: Couldn't connect to Docker daemon
```
$ docker-compose build
ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?
```
Please use `sudo` before any `docker-compose` command to make this work.
