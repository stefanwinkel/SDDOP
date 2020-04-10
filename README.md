# README SSDOP

SecureDockerDevOpsPipeline (SSDOP) is a series of demos/walkthrough how to configure tesing and usage of Docker in an automated environment. 

Docker these days is everywhere. From single Rasberry PIs, to Pods in Kubernetes Clusters in large corporate clouds. The amount of malware/vulernability discovered with Docker als keeps on increasing at an alarming rate. 

Usually we see Docker on Linux distributions, but in this demo session we are using a plain windows VM with Docker VM with the OpenSource Clair Vulnerability Scanner for CoreOS to scan Docker images for well known vulneabilties. We then scan a few well known images for vulnerabilities, including the latest NextCloud Docker image. We start from a plain Windows VM, and we see how can get quickly off the ground with just a few lines of Powershell. We launch CommandoVM framework from FireEye which includes Docker and various attacking tools are ported to run on Docker/Windows. It even includes a full Kali install !

As attackers/red teams are starting to beef up their game on Docker, it is even more important for Blue teams to start making sure that their Docker Images are free of malcious code.

Time to run this demo: Approx 20min


## SecureDoccerDevOpsPipeline - Clair Demo

### SDDOP Prerequisites

Plain Vanilla Window10 64bit with Powershell v5.1 (& TLS1.2). 

#### Demo use
Windows Education Preview, February 2020 build (build 19041.84)

### 1. Install BoxStarter, through elevated cmd prompt

```bash
md c:\script\sddop && cd c:\script\sddop && powershell.exe -exec bypass -C "iex ((New-Object System.Net.WebClient).DownloadString('https://boxstarter.org/bootstrapper.ps1')); Get-Boxstarter -Force
```

### 2. Install CommandoVM

```bash
powershell -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; (New-Object System.Net.WebClient).DownloadFile('https://github.com/fireeye/commando-vm/archive/master.zip', 'C:\script\master.zip');Expand-Archive -Path c:\script\master.zip -DestinationPath c:\script\sddop\Commando -Force "
```

### 3. Install SSDOP (SecureDockerDevOpsPipeline)

```bash
powershell -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; (New-Object System.Net.WebClient).DownloadFile('https://github.com/stefanwinkel/sddp/archive/master.zip', 'C:\script\master2.zip');$ProgressPreference='SilentlyContinue';Expand-Archive -Path c:\script\master2.zip -DestinationPath c:\script\sddop -Force " <NUL
```

### 4. Start the modifified CommandoVM / SSDOP installation

```bash 
cd c:\script\sddop\commando\commando-vm-master && powershell.exe -exec bypass .\install.ps1 -profile_file ..\..\SDDOP-master\sddop.json -nochecks 1 
```

### References

- [BoxStarter](https://boxstarter.org)
- [Chocolatey](https://chocolatey.org/docs/installation)
- [Windows Docker Machine](https://github.com/StefanScherer/windows-docker-machine)
- [CommandoVM](https://github.com/fireeye/commando-vm)
- [SDDOP SecureDockerDevOpsPipeline](https://github.com/stefanwinkel/sddop)