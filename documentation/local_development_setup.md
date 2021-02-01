# How to setup your local development environment using docker

## Create your local Repo for the development
Pull your **fork** Repo to your local folder.

```
[/]# cd /home/i336239
[/home/i336239]#
[/home/i336239]# git clone git@github.wdf.maga.corp:I336239/ansible-project.git
```
Then, all the codes in your fork Repo will be pulled into your local folder `/home/i336239/ansible-project`. Here, as an example, `/home/i336239/` is the local working directory.
```
[/home/i336239]# ls
ansible-project
```
For how to use GitHub/Git in detail, please refer to [git tips](documentation/git_tips.md).

## Download the docker image
There is a docker image which includes all requirements and dependencies we create for our Ansible project:
[storage-ansible-docker](https://github.wdf.maga.corp/Storage-Automation/storage-readymade).

You can simply download this image with:
```
# docker pull hub.global.cloud.maga/infrastructure_automation/storage-readymade:latest`
```
Then, you can list and confirm the image you downloaded.
```
# docker images
REPOSITORY                                                         TAG                 IMAGE ID            CREATED             SIZE
hub.global.cloud.maga/infrastructure_automation/storage-readymade   latest              d3e6fcc7873a        7 weeks ago         505MB
```

## Run a container with your local working directory mounted

Run a container using this docker image with command:
`docker run --name <container name> -it -v <your local working directory>:/data hub.global.cloud.maga/infrastructure_automation/storage-readymade:latest /bin/bash`
Options used:
`--name`  Assign a name to the container
`-v` Bind mount a volume
`-i` interactive, keep STDIN open even if not attached
`-t` tty, allocate a pseudo-TTY

Example:
```
[/]# docker run --name ansible_vivian_demo -it -v /home/i336239/:/data hub.global.cloud.maga/infrastructure_automation/storage-readymade:latest /bin/bash
[root@f73de4b4547bi:/ansible-project/ansible#
[root@f73de4b4547bi:/ansible-project/ansible#ls /
ansible-project  data             etc              lib              mnt              proc             run              srv              tmp              var
bin              dev              home             media            opt              root             sbin             sys              usr
[root@f73de4b4547bi:/ansible-project/ansible#ls /data
ansible-project  netapp           test
```

Now a container called `ansible_vivian_demo` is created. The container and the docker host share the same directory `/home/i336239` and this local working folder is available under `/data` in the container.
So when you change things locally in `/home/i336239` they will also be visible inside `/data` of the container.

Using `exit` to exit the container.

## Run the container in the background

If you find that the container has been stopped, you could restart the container using:
`[/]# docker start <container id or name>`

List running containers using `docker ps` or `docker container list`:
```
[/]# docker start ansible_vivian_demo
ansible_vivian_demo
[root@vml4427 /]# docker ps
CONTAINER ID        IMAGE                                                                     COMMAND                  CREATED             STATUS              PORTS                 NAMES
b68f650ac8da        hub.global.cloud.maga/infrastructure_automation/storage-readymade:latest   "/bin/bash"              5 minutes ago       Up 3 seconds                              ansible_vivian_demo
```

Next time, when you want to enter the container, use command `docker exec`:
```
[/]# docker exec -it ansible_vivian_demo bash
[root@b68f650ac8dai:/ansible-project/ansible#ls /data
ansible-project  netapp           test
[root@b68f650ac8dai:/ansible-project/ansible#exit
exit
[/]# docker ps
CONTAINER ID        IMAGE                                                                     COMMAND                  CREATED             STATUS              PORTS                 NAMES
b68f650ac8da        hub.global.cloud.maga/infrastructure_automation/storage-readymade:latest   "/bin/bash"              6 minutes ago       Up 43 seconds                             ansible_vivian_demo
```

After `docker exec`, even though you exit the container, it will be still running in the background.

## Change your Ansible configuration to point to your local fork Repo

By default, in the container, the work directory is officially `/ansible-project/ansible` which pulls the codes directly from the central Repo and is for production.
```
[/]# docker exec -it ansible_vivian_demo bash
[root@b68f650ac8dai:/ansible-project/ansible#ansible --version
ansible 2.8.2
  config file = /ansible-project/ansible/ansible.cfg
  configured module search path = ['/ansible-project/ansible/module']
  ansible python module location = /usr/lib/python3.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.7.3 (default, May  3 2019, 11:24:39) [GCC 8.3.0]
```
But for our own local development, we need to change the Ansible configuration to our local working directory.
Do following to change the config：
```buildoutcfg
[root@b68f650ac8dai:/ansible-project/ansible#cd /data/ansible-project/ansible/
[root@b68f650ac8dai:/data/ansible-project/ansible#export ANSIBLE_CONFIG=/data/ansible-project/ansible/ansible.cfg
```
Then check the config again:
```
[root@b68f650ac8dai:/data/ansible-project/ansible#ansible --version
ansible 2.8.2
  config file = /data/ansible-project/ansible/ansible.cfg
  configured module search path = ['/data/ansible-project/ansible/module']
  ansible python module location = /usr/lib/python3.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.7.3 (default, May  3 2019, 11:24:39) [GCC 8.3.0]
```
Now, we can find the config file and other relative search paths have been changed to our working directory under `/data`.

**Now with these above, you can use this container to do your development in your local working directory and play around Ansible.**