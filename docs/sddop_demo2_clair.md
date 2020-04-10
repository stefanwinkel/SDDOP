# SDDOP_2 - Demo 2: Docker Clair

SecureDockerDevOpsPipeline (SSDOP) is a series of demos/walkthroughs how to configure tesing and usage of Docker in an automated environment. 

Docker these days is everywhere. From single Rasberry PIs, to Pods in Kubernetes Clusters in large corporate clouds. The amount of malware/vulernability discovered with Docker also keeps on increasing at an alarming rate, with the latest Docker hitting the news earlier this week. 

Usually we see Docker on Linux distributions, but in this demo we are using a plain windows10 VM with Docker with the OpenSource Clair Vulnerability Scanner for CoreOS to scan Docker images for well known vulneabilties. We then scan a few well known images for vulnerabilities, including the latest NextCloud Docker image. We start from a plain Windows VM, and we see how can get quickly off the ground with just a few lines of Powershell. We launch CommandoVM framework from FireEye which includes Docker and various attacking tools are ported to run on Docker/Windows. It even includes a full Kali install !

As attackers/red teams are starting to beef up their game on Docker, it is even more important for Blue teams to start making sure that their Docker Images are free of malcious code.

## SSDOP Demo 2: Running Clair VM inside Docker container to Scan other Docker images

**Time to run: Approx 10min**

### Docker Vulernability Scanning with CoreOS Clair
```bash
cd /d c:\script\sddop
```

#### 1. Create Docker Network and a Volume
```bash
docker network create ci
docker volume create --name clair-postgres

```

#### 2. Start the Clair PostGress Database 
```bash
 docker run --detach --name clair-postgres --publish 5432:5432 --net ci  --volume clair-postgres:/var/lib/postgresql/data arminc/clair-db:latest

```

#### 3. Configure the clair service
```bash
curl --silent https://raw.githubusercontent.com/nordri/config-files/master/clair/config-clair.yaml | sed "s/POSTGRES_NAME/clair-postgres/" > config.yaml
```

### 4. Launch the Clair Container  
```bash
docker run --detach --name clair --net ci --publish 6060:6060 --publish 6061:6061 --volume c:\script\sddop\config.yaml:/config/config.yaml quay.io/coreos/clair:latest -config /config/config.yaml
```

#### 5. Pull down a few sample Dockers Images to be scanned
```bash 
docker pull debian:jessie &&  docker pull docker/docker-bench-security && docker pull nextcloud:apache
```

#### 6. Start the Clair Scanner
```bash
docker run -ti --rm --name clair-scanner --net ci -v /var/run/docker.sock:/var/run/docker.sock nordri/clair-scanner:latest /bin/bash

```

#### 7. Scan the Debian Jessie image from inside the Container
Inside the clair container:
```bash
export IP=$(ip r |tail -n1 |awk '{ print $9 }')
/clair-scanner --ip ${IP} --clair=http://clair:6060 debian:jessie
```

### 8. DevOps Pipeline Customization:  Clair Threshold
Inside the clair container:
```
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 debian:jessie
```

#### 9. A few more samples: MariaDB, NextCloud
```bash
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 nextcloud:apache
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 mariadb
```

### References

- [SecureDockerDevOpsPipeline (SDDOP)](https://github.com/stefanwinkel/sddop)