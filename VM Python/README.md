# Google Cloud VM Instance for Python
Create an VM-instance in the US-Central1 area and SSH into the machine

- Installing Python
```bash
sudo apt install python
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
```

