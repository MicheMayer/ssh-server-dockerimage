# Docker Definition for SSH Server
**Docker image creates a simple ssh server for testing purposes**

This server is not intended for a production environment. It is not configured with security in mind. Customize on your own.

### Build and run the container using docker cli
```sh
# Build the image
# --build-arg can be used to set a custom user and password
# Refer to the ARG entries defined in the dockerfile
# $cwd ssh-server-dockerimage
docker build -t local-ssh-server --build-arg user=myuser --build-arg pw=12345 .docker

# Startup the container
# Options:
# -rm Remove container after shutdown
# -d detach container from shell
# -p forward port
docker run --rm -d -p 22:22 local-ssh-server
```

### Retrieve the private key
```sh
scp myuser@localhost:~/.ssh/id_ed25519 /my/local/dir
#The authenticity of host 'localhost (::1)' can't be established.
#ECDSA key fingerprint is SHA256:**************************************************.
#Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
#Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
#myuser@localhost's password:
#...
#id_ed25519                                                                            100%  432   421.9KB/s   00:00
```

### Connect to the server using public-key authentication
```sh
# -i specifies the private keys location
# You can also use the clients ssh config file to register the key under a hostname
ssh -i /my/local/dir/id_ed25519 myuser@localhost
#myuser@e9682cac8fa4:~$
```

### Transfer file using RSync
The container definition also comes with rsync installed.
Windows users could use a client like cwrsync.

```sh
# Use the -e flag to specify the private key location, if you have not register the key in the ssh config.
# rsync -e "ssh -i /my/local/dir/id_ed25519" ...
rsync /source/location/dir myuser@localhost:~/destination/dir/
```
