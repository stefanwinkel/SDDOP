# SDDOP - Demo 1: Winconfig

SecureDockerDevOpsPipeline (SSDOP) is a series of demos/walkthroughs on how to build and use Docker Containers in a safe, secure environment. 

While this series mostly focuses on DevOps automation, many of the Docker concepts and items used/explained here, also apply to any container and orchestration services at large, besides Docker and Kubernetes (K8S).

Docker containers these days are everywhere. From single Rasberry PIs to K8S Clusters as part of large corporate clouds. The amount of malware/vulernability discovered with Docker keeps on increasing at alarming speeds. The latest Docker malware, Kinsing, was just discovered earlier this week (03/05/20) and had been running for months! With modern cloud development, like statelesss services and containers running everywhere, due to contineously changing production environments as as result of orchestration services like K8S where containers might live for just a few seconds, it makes it increasing complex for organizations / blue teams to monitor and track vulnerabilities and exploits. 

As attackers/red teams are starting to beef up their game on Docker with distributions like CommandoVM and Kali, it is now even more important for Blue teams to start making sure that their Docker environments are being monitored correctly and are safe from these type of issues.

## Demo Intro

We start from a plain Windows10 VM, and we see how can get quickly off the ground with just a few lines of Powershell. We launch CommandoVM framework from FireEye which includes Docker and various attacking tools are ported to run on Docker/Windows. It even includes a full Kali install. Within a few minutes we have a full working Windows10 Docker environment.

## SSDOP Demo 1: Install Docker and various SecurityTools onto Windows10-64bit 

- Time to run: Approx 20min
- Description: Shows how we (and attackers) can quickly can spin up Docker containers in seconds, even from a plain Win10 environment. 
- Pre-reqs: Plain Win10 64bit OS, with Powershell 5.1 (TLS1.2) enabled. Network access recommended.

## SSDOP Demo 1: Install Docker and other Tools

**Time to run: Approx 20min**

### SDDOP Prerequisites

Plain Vanilla Window10 64bit with Powershell v5.1 (& TLS1.2). 

#### SDDOP Demo details 
Win10 Education Preview, February 2020 build (build 19041.84)

### 1. Install BoxStarter, through elevated cmd prompt 

```bash
md c:\script\sddop && cd c:\script\sddop && powershell.exe -exec bypass -C "iex ((New-Object System.Net.WebClient).DownloadString('https://boxstarter.org/bootstrapper.ps1')); Get-Boxstarter -Force
```

### 2. Download CommandoVM Package

```bash
powershell -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; (New-Object System.Net.WebClient).DownloadFile('https://github.com/fireeye/commando-vm/archive/master.zip', 'C:\script\master.zip');Expand-Archive -Path c:\script\master.zip -DestinationPath c:\script\sddop\Commando -Force "
```

### 3. Download SSDOP (SecureDockerDevOpsPipeline)

```bash
powershell -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; (New-Object System.Net.WebClient).DownloadFile('https://github.com/stefanwinkel/sddp/archive/master.zip', 'C:\script\master2.zip');$ProgressPreference='SilentlyContinue';Expand-Archive -Path c:\script\master2.zip -DestinationPath c:\script\sddop -Force " <NUL
```

### 4. Start the SDDOP install based upon modified CommandoVM

```bash 
cd c:\script\sddop\commando\commando-vm-master && powershell.exe -exec bypass .\install.ps1 -profile_file ..\..\SDDOP-master\sddop.json -nochecks 1 
```

### References

- [SecureDockerDevOpsPipeline (SDDOP)](https://github.com/stefanwinkel/sddop)