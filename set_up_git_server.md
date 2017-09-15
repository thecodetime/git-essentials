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
