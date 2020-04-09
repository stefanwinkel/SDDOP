# CLAIR_DEMO: Securing The Docker DevOps Pipeline 

### Docker Vulernability Scanning with CoreOS Clair

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
curl --silent https://raw.githubusercontent.com/nordri/config-files/master/clair/config-clair.yaml | sed "s/POSTGRES_NAME/clair-postgres/" > config.yaml


### 4. Launch the Clair Container  
```bash
docker run --detach --name clair --net ci --publish 6060:6060 --publish 6061:6061 --volume c:\script\clair\config.yaml:/config/config.yaml quay.io/coreos/clair:latest -config /config/config.yaml
```

#### 5. Pull down a few sample Dockers Images to be scanned
```bash 
docker pull debian:jessie && docker pull 
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

### DevOps Pipeline Customization:  Clair Threshold
Inside the clair container:
```
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 debian:jessie
```

#### A few more Scanning Examples: MariaDB, NextCloud
```bash
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 nextcloud:apache
/clair-scanner --ip ${IP} --threshold "Critical" --clair=http://clair:6060 mariadb
```
