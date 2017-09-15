# Connect to Remote box with Private key (PEM)

### Prerequisites

1. A remote server like DigitalOcean (already enabled root access with SSH)
2. SSH client

## Generate PEM file

1. Generate ssh key on local machine. Enter any name of the SSH key file `digito`. Enter through passphrase (if set, password will be needed everytime) 

```bash
$ ssh-keygen -t rsa -b 2048 -v

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/ek/.ssh/id_rsa): digito
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in digito.
Your public key has been saved in digito.pub.
The key fingerprint is:
...
```

2. There are 2 files generated in local directory `digito` and  `digito.pub`
3. Rename `digito` to `digito.pem`
4. Copy public key over to the remote server. (replace *XXX* with the server IP)

```bash
$ ssh-copy-id -i digito.pub root@XXX.XXX.XXX.XXX
```

5. To check if the key is already added into the server

```bash
$ ssh root@XXX.XXX.XXX.XXX
$ cat ~/.ssh/authorized_keys
```

**Notes:**
Adding authorized keys on remote server can be done manually by creating `~/.ssh/authorized_keys` with 600 permission and basically copy & paste a content of remote public keys (id_rsa.pub, digito.pub) to it.


## Create new account
You should always use another account for specific purpose and use `root` as minimal as possible. This is a common security guidance for all servers.

Create a user named `deploy` for deployment tasks
```bash
# adduser deploy
# usermod -aG sudo deploy   //note: add 'deploy' user to sudo group
```

## Ease SSH access with the config
1. On local machine, setting ssh config to access the server with the PEM. Edit `.ssh/config` with the settings. (`deploy` is the server user, `digito.pem` is the key we generated on local, replace XXX with the real server IP)
```bash
$ vi ~/.ssh/config

Host thecodetime
    User deploy
    HostName XXX.XXX.XXX.XXX
    IdentityFile ~/.ssh/digito.pem
```

2. Easily access the remote server with `deploy` user.
```bash
$ ssh thecodetime
```
