# Create Git Server for Deployment

### Prerequisites

1. A remote server like DigitalOcean (already enabled root access with SSH)
2. SSH client

## Git Server

1. Connect to server with SSH (`deploy` user) 
2. Create Git bare repository (no working directories). We need only the bare git as we will checkout codes to different location for server root path later.

```bash
$ pwd
/home/deploy
$ git init --bare production_codetime.git
```
3. Create `post-receive` hook which is executed after a code push.
```bash
$ cd production_codetime.git/hooks
$ touch post-receive
$ chmod +x post-receive
```

4. Fill out `post-receive` with following settings.

```
#!/bin/sh

## store the arguments given to the script
read oldrev newrev refname

GREEN="\033[0;32m"
NOCOLOR='\033[0m'
echo "${GREEN}Received Push Request at $( date +'%d/%m/%y %T %Z' )${NOCOLOR}"

WORK_TREE='/home/deploy/server/production/codetime'
git --work-tree=$WORK_TREE checkout -f

# build
cd $WORK_TREE
yarn
node_modules/.bin/gulp build
```

The `WORK_TREE` is a web server directory. There is a command `git checkout ... -f` to that working directory with `force`. This mean whenever a code is pushed to this git repository. It will checkout the recent codes to the working directory and runs some scripts in it.

5. Create directories for the web server (which is the root path for nginx)
```bash
$ cd
$ mkdir -p server/production/codetime
```

6. Make sure the firewall is on and allows SSH and HTTP.
```bash
$ sudo ufw app list
$ sudo ufw allow 'OpenSSH'
$ sudo ufw allow 'Nginx HTTP'
$ sudo ufw enable
$ sudo ufw status
```

7. Setup Nginx and set root path to `WORK_TREE` directory.
```bash
# in case Nginx is not installed
$ sudo apt-get update
$ sudo apt-get install nginx

$ sudo vi /etc/nginx/sites-enabled/default
```

__default__
```
# forward all non-www to www
server {
        listen 80;
        server_name thecodetime.com;
        return 301 http://www.thecodetime.com$request_uri;
}
server {
        listen 80;
        listen [::]:80;
        server_name www.thecodetime.com;

        root /home/deploy/server/production/codetime;
        index index.html;
        location / {
                try_files $uri $uri/ =404;
        }
}
```

8. Restart Nginx and browse to [http://www.thecodetime.com](http://www.thecodetime.com).
```bash
# to start Nginx
$ sudo nginx

# to restart
$ sudo nginx -s reload
```



