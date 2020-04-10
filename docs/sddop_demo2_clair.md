# SDDOP - Demo 2: Docker Clair

SecureDockerDevOpsPipeline (SDDOP) is a series of demos/walkthroughs on how to build and use Docker Containers in a safe, secure environment. 

While this series mostly focuses on DevOps automation, many of the Docker concepts and items used/explained here, also apply to any container and orchestration services at large, besides Docker and Kubernetes (K8S).

Docker containers these days are everywhere. From single Rasberry PIs to K8S Clusters as part of large corporate clouds. The amount of malware/vulernability discovered with Docker keeps on increasing at alarming speeds. The latest Docker malware, Kinsing, was just discovered earlier this week (03/05/20) and had been running for months! With modern cloud development, like statelesss services and containers running everywhere, due to contineously changing production environments as as result of orchestration services like K8S where containers might live for just a few seconds, it makes it increasing complex for organizations / blue teams to monitor and track vulnerabilities and exploits. 

As attackers/red teams are starting to beef up their game on Docker with distributions like CommandoVM and Kali, it is now even more important for Blue teams to start making sure that their Docker environments are being monitored correctly and are safe from these type of issues. 

## Demo Intro

Usually we see Docker on Linux distributions, but in this demo we are using a plain windows10 VM with Docker with the OpenSource Clair Vulnerability Scanner for CoreOS to scan Docker images for well known vulneabilties. We scan a few well known Docker images for vulnerabilities, including the latest NextCloud Docker image. We start from a plain Windows VM that has our SDDOP package installed and have the Clair Scanner up with just a few lines of Powershell. 

## SDDOP Demo 2: Running Clair VM inside Docker container to Scan other Docker images

- Time to run: Approx 10min
- Description: A vulnerabilty scan with the FOSS tool Clair, from CoreOS, shows that even popular Docker Images like NextCloud contain well known vulnerabilities.
- Pre-reqs: SDDOP package installed on Win10, or any other OS with latest version of Docker. Network access to DockerHub required

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

#### 8. DevOps Pipeline Customization:  Clair Threshold
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