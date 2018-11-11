---
layout: post
title: CE-EE node activate
---
November 8,2018 was a great day for Docker community as we saw new stable release for community edition Docker 18.09 CE afer moving to the new release and support cadence.This will be supported for 7 months with Docker 19.03 CE being the next release in line.

Along with this being an Enterprise customer we are excited to have [Docker Enterprise 2.1](https://blog.docker.com/2018/11/introducing-docker-enterprise-2-1-advancing-our-container-platform-leadership/) which adds support for Windows Server 1709, 1803 and Windows Server 2019. Read more about this in the [blog](https://blog.docker.com/2018/11/introducing-docker-enterprise-2-1-advancing-our-container-platform-leadership/) from Docker team.

The one feature I was keen to test on was upgrading CE engine to EE engine using the Docker cli. Here are the steps I followed to upgrade a CentOS machine running 17.12.0-ce engine to 18.09 (EE engine).

{% highlight console %}
$ docker version
Client:
 Version:	17.12.0-ce
 API version:	1.35
 Go version:	go1.9.2
 Git commit:	c97c6d6
 Built:	Wed Dec 27 20:10:14 2017
 OS/Arch:	linux/amd64

Server:
 Engine:
  Version:	17.12.0-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.2
  Git commit:	c97c6d6
  Built:	Wed Dec 27 20:12:46 2017
  OS/Arch:	linux/amd64
  Experimental:	false
{% endhighlight %}

Lets add the stable repository so that we have the latest pacakges to install the new release of Docker CE.

{% highlight console %}
$ sudo yum-config-manager \
     --add-repo https://download.docker.com/linux/centos/docker-ce.repo
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
{% endhighlight %}

To install specific version of Docker instead of always using the latest, firt list the available versions.

$ sudo yum list docker-ce --showduplicates 

{% highlight console %}
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
docker-ce-stable                                      | 3.5 kB  00:00:00     
(1/2): docker-ce-stable/x86_64/updateinfo             |   55 B  00:00:00     
(2/2): docker-ce-stable/x86_64/primary_db                19 kB  00:00:00     
Installed Packages
docker-ce.x86_64        17.12.0.ce-1.el7.centos       @docker-ce-stable
Available Packages
docker-ce.x86_64        17.03.0.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.03.1.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.03.2.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.03.3.ce-1.el7              docker-ce-stable 
docker-ce.x86_64        17.06.0.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.06.1.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.06.2.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.09.0.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.09.1.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.12.0.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        17.12.1.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        18.03.0.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        18.03.1.ce-1.el7.centos       docker-ce-stable 
docker-ce.x86_64        18.06.0.ce-3.el7              docker-ce-stable 
docker-ce.x86_64        18.06.1.ce-3.el7              docker-ce-stable 
docker-ce.x86_64        3:18.09.0-3.el7               docker-ce-stable 
{% endhighlight %}

The standard way of installing a specific version is `$ sudo yum install <FULLY-QUALIFIED-PACKAGE-NAME>`

> ***The version string is the package name plus the version up to the first hyphen. In the example above, the fully qualified package name is docker-ce-18.09.0***

Let's upgrade Docker to 18.09

 {% highlight console %}
$ sudo yum install docker-ce-18.09.0
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Resolving Dependencies
--> Running transaction check
---> Package docker-ce.x86_64 0:17.12.0.ce-1.el7.centos will be updated
---> Package docker-ce.x86_64 3:18.09.0-3.el7 will be an update
--> Processing Dependency: containerd.io for package: 3:docker-ce-18.09.0-3.el7.x86_64
--> Processing Dependency: docker-ce-cli for package: 3:docker-ce-18.09.0-3.el7.x86_64
--> Running transaction check
---> Package containerd.io.x86_64 0:1.2.0-3.el7 will be installed
---> Package docker-ce-cli.x86_64 1:18.09.0-3.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================================================================================================
 Package                                    Arch                                Version                                         Repository                                     Size
====================================================================================================================================================================================
Updating:
 docker-ce                                  x86_64                              3:18.09.0-3.el7                                 docker-ce-stable                               19 M
Installing for dependencies:
 containerd.io                              x86_64                              1.2.0-3.el7                                     docker-ce-stable                               22 M
 docker-ce-cli                              x86_64                              1:18.09.0-3.el7                                 docker-ce-stable                               14 M

Transaction Summary
====================================================================================================================================================================================
Install             ( 2 Dependent packages)
Upgrade  1 Package

Total download size: 55 M
Is this ok [y/d/N]: y
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/3): containerd.io-1.2.0-3.el7.x86_64.rpm                                                                                                                  |  22 MB  00:00:07     
(2/3): docker-ce-18.09.0-3.el7.x86_64.rpm                                                                                                                    |  19 MB  00:00:09     
(3/3): docker-ce-cli-18.09.0-3.el7.x86_64.rpm                                                                                                                |  14 MB  00:00:03     
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                               5.1 MB/s |  55 MB  00:00:10     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Warning: RPMDB altered outside of yum.
  Installing : containerd.io-1.2.0-3.el7.x86_64                                                                                                                                 1/4 
  Installing : 1:docker-ce-cli-18.09.0-3.el7.x86_64                                                                                                                             2/4 
  Updating   : 3:docker-ce-18.09.0-3.el7.x86_64                                                                                                                                 3/4 
  Cleanup    : docker-ce-17.12.0.ce-1.el7.centos.x86_64                                                                                                                         4/4 
  Verifying  : 1:docker-ce-cli-18.09.0-3.el7.x86_64                                                                                                                             1/4 
  Verifying  : 3:docker-ce-18.09.0-3.el7.x86_64                                                                                                                                 2/4 
  Verifying  : containerd.io-1.2.0-3.el7.x86_64                                                                                                                                 3/4 
  Verifying  : docker-ce-17.12.0.ce-1.el7.centos.x86_64                                                                                                                         4/4 

Dependency Installed:
  containerd.io.x86_64 0:1.2.0-3.el7                                                      docker-ce-cli.x86_64 1:18.09.0-3.el7                                                     

Updated:
  docker-ce.x86_64 3:18.09.0-3.el7                                                                                                                                                  

Complete!
{% endhighlight %}

 There are certain requirements to activate the EE engine from a CE:
 1. The Docker Community Edition (CE) version must be 18.09 or higher.
 2. All of the Docker packages must be installed: docker-cli, docker-server, and containerd.
 3. A valid Docker EE license (obtain it from the Docker Hub under your organization profile)

 At this point this feature has some limitations:
 1. Only supported on x86 Linux nodes.
 2. Windows nodes are not currently supported.
 3. Node-level Engine activation between CE and EE is only supported in the same version of Docker Enterprise Engine for Docker.
 4. Prior version of Docker CE do not support this feature


Considering the above requirements and limitation lets check the version of Docker to ensure that we can do a CE-EE node activation

{% highlight console %}
$ docker version
Client:
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:48:22 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.0
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.4
  Git commit:       4d60db4
  Built:            Wed Nov  7 00:19:08 2018
  OS/Arch:          linux/amd64
  Experimental:     false
 {% endhighlight %}

> NOTE: When running the command docker login, the shell stores the credentials in the current user’s home directory. RHEL and Ubuntu-based Linux distributions have different behavior for sudo. RHEL sets $HOME to point to /root while Ubuntu leaves $HOME pointing to the user’s home directory who ran sudo and this can cause permission and access problems when switching between sudo and non-sudo’d commands.

> This is something I missed during my steps and thanks to Docker team for helping me out in resolving this issue.

Here is the new Management command available in this release:

{% highlight console %}
$ sudo docker engine --help
Usage:	docker engine COMMAND

Manage the docker engine

Commands:
  activate    Activate Enterprise Edition
  check       Check for available engine updates
  update      Update a local engine
{% endhighlight %}

Make sure you have downloaded your license file from Docker Store to activate the license using the command below:

{% highlight console %}
$ sudo docker engine activate --license docker_subscription.lic 
[sudo] password for sujaypillai: 
License: Quantity: 20 Nodes	Expiration date: 2019-07-30	License is currently active
18.09.0: resolving 
unable to pull image docker.io/store/docker/engine-enterprise-dm:18.09.0: failed to resolve reference "docker.io/store/docker/engine-enterprise-dm:18.09.0": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
{% endhighlight %}

>> The reason this failed is because you need to login to your dockerhub using the same userid which you used to download the license file.

{% highlight console %}
$ sudo docker login docker.io -u sujaypillai
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
{% endhighlight %}

Lets activate the EE license 

{% highlight console %}
$ sudo docker engine activate --license docker_subscription.lic 
License: Quantity: 10 Nodes	Expiration date: 2019-05-25	License is currently active
18.09.0: resolved 
8426d869cd15: done [==================================================>]     433B/433B
04943fdcc3c7: done 
130b7807ec4a: done 
3470a7591bbb: done [==================================================>]  4.594kB/4.594kB
15bd6c72f8d4: done 
22797e024efe: done [==================================================>]    850kB/850kB
Successfully activated engine.
Restart docker with 'systemctl restart docker' to complete the activation.
{% endhighlight %}


Ohh... there are some cool features available with this release

### BuildKit
[BuildKit](https://blog.mobyproject.org/introducing-buildkit-17e056cc5317) is a new build system currently available with the existing Dockerfile builder. To make use of the new build mechanism all you need to do is set the environment variable DOCKER_BUILDKIT. Below is comparison between the normal Dockerfile builder and BuildKit.

{% highlight console %}
$ docker build -t sujaypillai/vsts-agent .
Sending build context to Docker daemon   2.56kB
Step 1/6 : FROM microsoft/vsts-agent:ubuntu-16.04
 ---> 5fb5635c1171
Step 2/6 : ENV DOCKER_CHANNEL test
 ---> Using cache
 ---> 0cfa5eef1785
Step 3/6 : ENV DOCKER_VERSION 18.09.0-ce-beta1
 ---> Using cache
 ---> fc3b8cd9f926
Step 4/6 : RUN set -ex  && curl -fL "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/`uname -m`/docker-${DOCKER_VERSION}.tgz" -o docker.tgz  && tar --extract --file docker.tgz --strip-components 1 --directory /usr/local/bin  && rm docker.tgz  && docker -v
 ---> Using cache
 ---> 6efb1179a5a0
Step 5/6 : ENV DOCKER_COMPOSE_VERSION 1.22.0
 ---> Using cache
 ---> 79b47efa12fd
Step 6/6 : RUN set -x  && curl -fSL "https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-`uname -s`-`uname -m`" -o /usr/local/bin/docker-compose  && chmod +x /usr/local/bin/docker-compose  && docker-compose -v
 ---> Using cache
 ---> 08b526aafb58
Successfully built 08b526aafb58
Successfully tagged sujaypillai/vsts-agent:latest

$ export DOCKER_BUILDKIT=1

$ docker build -t sujaypillai/vsts-agent .
[+] Building 47.7s (7/7) FINISHED                                                                                                                                                   
 => [internal] load build definition from Dockerfile                                                                                                                           0.4s
 => => transferring dockerfile: 696B                                                                                                                                           0.0s
 => [internal] load .dockerignore                                                                                                                                              0.5s
 => => transferring context: 2B                                                                                                                                                0.0s
 => [internal] load metadata for docker.io/microsoft/vsts-agent:ubuntu-16.04                                                                                                   0.0s
 => [1/3] FROM docker.io/microsoft/vsts-agent:ubuntu-16.04                                                                                                                     0.7s
 => => resolve docker.io/microsoft/vsts-agent:ubuntu-16.04                                                                                                                     0.0s
 => [2/3] RUN set -ex  && curl -fL https://download.docker.com/linux/static/test/`uname -m`/docker-18.09.0-ce-beta1.tgz -o docker.tgz  && tar --extract --file docker.tgz --  30.0s
 => [3/3] RUN set -x  && curl -fSL https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose  && chmo  14.4s
 => exporting to image                                                                                                                                                         1.7s
 => => exporting layers                                                                                                                                                        1.4s
 => => writing image sha256:93dd0d4743cd68106c390dbecbc35fdff05798513b0128aa159c9b8669420e14                                                                                   0.0s
 => => naming to docker.io/sujaypillai/vsts-agent   
 {% endhighlight %}

> You can also set the feature option in /etc/docker/daemon.json to enable BuildKit by default: 
{% highlight json %}
  { “features”: { “buildkit”: true } }
{% endhighlight %}