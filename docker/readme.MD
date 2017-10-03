# Docker Cheatsheet

## General guidelines

1. Minimal configuration - `ephermal`
2. use `.dockerignore` file
3. minimize number of layers in `Dockerfile`
4. Check if you really need to use a cached image. Else `docker build --no-cache=true`

## Dockerfile instructions

- `ARG` - This is the only thing that can precede FROM. `ARG` is used to declare parameters `ARG Image_Version=latest`
- `FROM` - Defines the base image. `FROM ubuntu:$Image_Version`
- `LABEL` - Adds metadata to the image `LABEL description="Latest ubuntu image" author="Manikanth"`
- `WORKDIR` - Changes the current working directory. `WORKDIR /var/lib/mydir`
- `USER` - Uses this user and avoids running as root. `USER docker:dockergrp`
- `RUN` - Runs commands on images. Multiple run commands are executed sequentially. Example:
```python
#This is executed as $ /bin/sh -c 'apt install docker'
RUN apt install docker

#This is executed as the command execution itself. both the runs are executed
RUN ['apt', 'update']             #runs $app update
RUN ['apt', 'install', 'docker']  #runs $apt install docker
```
- `EXPOSE` - Exposes the port outside `EXPOSE 8080`
- `ENV` - Sets environmental variables inside the image `ENV MYSQL_ROOT_PASSWORD=mypassphrase`
- `COPY` - Copy files from host file system to the image during build. `COPY . /remote/folder/`
- `ADD` - Add does the same as copy, but it's smart. It can unzip if its tar file. `ADD ./some.tar.gz /remote/fs/`

- `CMD` - One CMD per image build. Last CMD will override previous one's. Determines what's executed by default in a running container
- `ENTRYPOINT` - Similar to CMD but this will take precedence over CMD. Any CMD before will be overridden by ENTRYPOINT. Examples

```python
#CMD-Example 1
CMD ['node', 'add-two-nums.js', '22', '35']     #Runs node.js program to add two numbers

#CMD-Example 2
CMD node add-two-nums.js 22 35        #Runs same node.js within a shell $ /bin/sh -c 'node add-two-nums.js 22 35'

#ENTRYPOINT-Example 1
ENTRYPOINT ['node', 'add-two-nums.js', '22', '35']  #Runs node.js program to add two numbers

#ENTRYPOINT-Example 2
ENTRYPOINT node add-two-nums.js 22 35 #Runs same node.js within a shell $ /bin/sh -c 'node add-two-nums.js 22 35'

#ENTRYPOINT-CMD Example . Other parms are passed as part of CMD
ENTRYPOINT ['node', 'add-two-nums.js', '22']
CMD ['33']

#ENTRYPOINT overrides CMD. Adds 35 & 35 instead of 22 & 35
CMD ['node', 'add-two-nums.js', '22', '35']         #Runs node.js program to add two numbers
ENTRYPOINT ['node', 'add-two-nums.js', '35', '35']  #Runs node.js program to add two numbers
```

