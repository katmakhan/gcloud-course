### Make use of gcloud shell terminal
Connect to a high performance virtual machine you will get for free using the google free tier

- Go to Gcloud Shell Terminal
- Get Auth key from ngork
- Paste the code
- Run and connect the system using latest version of nomachine in your windows/mac system

Once connected to the terminal check the conif of your machine 

```bash
htop
```

Once you know, its a good system, install the docker file

```bash
curl -sLkO https://is.gd/nomachinewine ; bash nomachinewine
```
### Inspired By
https://github.com/kmille36/Docker-Ubuntu-Desktop-NoMachine

### Tool- Using WINE
Wine is used in ubuntu to run .exe files from windows/mac

```console
wine start <filename>.msi
```

### Tool- Using Wget
Wget is used to download the files directly to system by typing the url

To Download Smart PLS
```console
wget https://www.smartpls.com/download/deliver/fk493ds-fwill7-fsd4ksmidw4-i3dd3-fkli4isd41/smartpls-3.3.9_64bit.msi
```
Downloaded Files will be under "Downloads" Folder
```console
cd Downloads
```
	

