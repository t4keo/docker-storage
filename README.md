# Docker Storage
**This is my second attempt at making a docker and even an automated-build one. This is mostly a Cheatsheet for myself but if it helps you I'm glad**

It creates a debian/jessie storage container.
I created it to work with my Apache2/Php5 container but I'm sure I will use it in many other ways (e.g. the one I made - [GitHub : t4keo/apache-php](https://github.com/t4keo/docker-apache-php-mail) - [Docker : t4keo/apache-php](https://hub.docker.com/r/t4keo/apache-php-exim/)).

## How to launch it
To launch this docker you just have to use one command in order to make it work in background. Since it's a storage container, make sure that you launch it before the apache-php container and even before any container that needs data from it :

``` sh
$ docker run -id --name storage t4keo/storage bash
```
* The -id is used to launch the container in interactive and detached mode
* the --name option is used to give a comprehensive name to the container

If you want to enter in the container to work on it, just use the command below :

``` sh
$ docker exec -it storage bash
```
* -it is used to enter interactive mode and show the terminal

## How it works ?
The volume linked to the others is the /home so if you want to use the datas that you put in in an other folder you just have to create a symbolic link.

E.g. I want my website in my apache-php container but how can I do it ?
First I place my website in a folder called "html"

``` sh
$ docker exec -it storage bash
$ mkdir /home/html
$ exit
```
* Enter in the storage container
* Create the html folder
* Exit the container

Then I locate where is this created folder
``` sh
$ docker inspect storage
```
Which tells me something like 
``` sh
...
 "Volumes": {
        "/home": "/var/lib/docker/volumes/803a45ba......e6c52d947/_data"
    },
    "VolumesRW": {
        "/home": true
    },
...
```

I then enter in this folder using WinSCP (or any FTP client) in SFTP and place all the files and folders of my website.

And voila

## How to remove this container
First make sure that you have stopped all the other running container linked to this one (safety's first) and stop it
``` sh
$ docker stop [linkded-container name]
...
$ docker stop storage
```
Then remove it using the -v option which will delete the associated volume so it clears up the space it tooks at creation and with all the stored data in this basic storage container
``` sh
$ docker rm -v storage
```
