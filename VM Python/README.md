# Google Cloud VM Instance for Python
Create an VM-instance in the US-Central1 area and SSH into the machine

- Installing Python
```bash
sudo apt install python
```
- Installing Python3
```bash
sudo apt install python3
```
- Installing PIP
```bash
sudo apt install pip
```
```bash
sudo apt install pip3
```
- Installing git
```bash
sudo apt install git
```

- Make new Dir
```bash
mkdir newfolder
```

- Move into new Dir
```bash
cd newfolder
```

- Settingup GIT
	```bash
	git init
	```
	```bash
	git remote add origin "https://-----.git"
	```
	```bash
	git pull origin master
	```

	- The password doesn't work, so create a personel token inside github
	- From 2021-08-13, GitHub is no longer accepting account passwords when authenticating Git operations. You need to add a PAT (Personal Access Token) instead
	- From your GitHub account, go to Settings → Developer Settings → Personal Access Token → Generate New Token (Give your password) → Fillup the form → click Generate token → Copy the generated Token, it will be something like ghp_sFhFsSHhTzMDreGRLjmks4Tzuzgthdvfsrta


- Intalling via requirements
```bash
pip install -r requirements.txt
or 
pip3 install -r requirements.txt
```
- Chaning the default python
```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
```
- Installing TMUX for terminal control
```bash
 sudo apt install tmux
 ```
## TMUX COMMANDS
These are some of the tmux commands that you want to know, to preserve the state of multiple terminal instances.
- Create new tmux session with name
```bash
tmux new -s sessionname
```
- Renaming session
```bash
tmux rename-session -t oldname newname
```
- Listing all tmux sessions
```bash
tmux ls
```
- Acessing a session
```bash
tmux attach -t sessionname
```
- Killing a session
```bash
tmux kill-session -t sessionname
```
- Kill every tmux session
```bash
tmux kill-server
```

- Some of the keyboard shortcuts are:
	- Dettach from session
	```
	Keybord shortcut : crt+b d
	```
	- Rotating the pane
	```
	Keybord shortcut : crt+b o
	```
	- Move inside pane
	```
	Keybord shortcut : crt+b ;
	```
	- Split in Vertical
	```
	Keybord shortcut : crt+b %
	```
	- Split in Horizontal
	```
	Keybord shortcut : crt+b ''
	```
	- Clear screen
	```
	Keybord shortcut : crt+l
	```
	- To kill the session pane
	```
	Keybord shortcut : crt+b x
	```


## Main Issues
- If pip not working, try updating the packet manager
- If not working try, updating the PiP when requirements.txt aren't fully installed. Issue is seen with Ubuntu 18.04, Python 3.6.9 in the GCP VM Instance
- Stuck at grpcio

## Troubleshooting
- Before install 3.7, we should have to install python 3.6 by running the following command.
```bash
sudo apt-get install python3.6
sudo apt-get install python3.7
```
- Follow some of the trouble shooting tips
	- Upgrade the apt get and Install the new python
	```bash
	sudo apt-get update && sudo apt-get upgrade
	sudo apt install python3.7
	```
	- Upgrade pip- This works most of the time, when stuck at grpcio
	```bash
	pip3 install --upgrade pip
	```	
	- Uninstall and install pip
	```bash
	sudo apt-get remove python3-pip
	sudo apt-get install python3-pip
	```
	- Downgrade setuptools
	```bash
	python3 -m pip install --upgrade setuptools==49.6.0
	```
	- Installing grpcio seperately, as 1.43.0 maynot be working, 1.48.2 is not working
	```bash
	pip3 install grpcio==1.39.0
	```
	- Installing without using cache
	```bash
	pip3 install -r requirements.txt --no-cache-dir
	```
	- Change the python version from 3.6 to 3.7
	```bash
	sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
	```
