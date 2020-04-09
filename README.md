# README SSDOP

## SecureDoccerDevOpsPipeline - Clair Demo

### SDDOP Prerequisites

Plain Vanilla Window10 with Powershell (& TLS1.2). 

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

### 4. Start the installation

```bash 
cd c:\script\sddop\commando && powershell.exe -exec bypass .\install.ps1 -profile_file ..\ssdop_profile.json -nochecks 1 -password sddop
```

### References

- [BoxStarter](https://boxstarter.org)
- [Chocolatey](https://chocolatey.org/docs/installation)
- [Windows Docker Machine](https://github.com/StefanScherer/windows-docker-machine)
- [CommandoVM](https://github.com/fireeye/commando-vm)
- [SDDOP SecureDockerDevOpsPipeline](https://github.com/stefanwinkel/sddop)