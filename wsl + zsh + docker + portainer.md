# Install `wsl`+ `zsh`+`docker`+`portainer`


## 1 – Dowload windows terminal if not installed


You can dowload it on:

[https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=nl-nl&gl=nl&rtc=1](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=nl-nl&gl=nl&rtc=1)

## 2 – Set up wsl 2

Check if you have it running:
```bash
wsl –status
```
If not pick a distor in the list below, running:
```bash
wsl --list –online
```
Install wsl2 with picked distro
```bash
wsl --install ubuntu
```
## 3 – Add to windows terminal

<kbd>Windows Terminal</kbd> > <kbd>Settings</kbd>  > <kbd>Open JSON File</kbd>
```json
{  
"commandline": "C:\WINDOWS\system32\wsl.exe -d Ubuntu",

"colorScheme": "One Half Dark",  
"guid": "{aaaaaaaaaaaaaaaaa-aaaaaaaaaa-aaaaaaaa}",  
"hidden": false,  
"name": "Ubuntu",  
"source": "Windows.Terminal.Wsl",  
"startingDirectory": "C:\\Users\\corentink"  
}
```
## 4 – Install `OhMyZSH`

  
```bash
sudo apt update

sudo apt install git zsh -y
```
Finally, we’ll go ahead and setup `oh my zsh`. Let’s start with setting up the prerequisites:

```bash
sudo apt update

sudo apt install git zsh -y
```  
With the prerequisites installed, we can go ahead and install Oh my zsh:
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
This will ask you if you want the switch your shell to zsh. Hit yes.

## 5 – Pick a theme

  

[https://github.com/ohmyzsh/ohmyzsh/wiki/Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

To change the theme, edit the `~/.zshrc` file and input the picked theme there.

## 6 – Install docker

  
```bash
sudo apt update

sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```
  

Next, run the commands below to download and install Docker’s official GPG key. The key is used to validate packages installed from Docker’s repository making sure they’re trusted.

  
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88
```
Now that the official GPG key is installed, run the commands below to add its stable repository to Ubuntu. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below.

  
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
After this command, Docker’s official GPG and repository should be installed on Ubuntu.

If you have older versions of Docker, run the commands below to remove them:
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
When you have removed all the previous versions of Docker, run the commands below to install the latest and current stable version of Docker:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
This will install Docker software on Ubuntu.

Add your account, for most cases it will be ubuntu, to Docker group and restart:
```bash
sudo systemctl enable --now docker

sudo usermod -aG docker $USER
```
Reboot your instance:
```bash
sudo reboot
```
To verify that Docker CE is installed correctly you can run the hello-world image:
```bash
sudo docker run hello-world
```

## 7 - Install docker-compose
Dowload the Docker Compose binary github
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/2.15.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
After downloading it, run the commands below to get it running on WSL

 ```bash
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
To test it, try:
 ```bash
docker-compose --version
 ```
  

## 8 - Set up a swarm
Check the IP address of  machine with `docker0`:
```bash
ip a
```
Initialize the Swarm with the command:
```bash
docker swarm init --advertise-addr IP
```
with the IP of `docker0`

When that command completes, it’ll print out a join command that looks like this:
```bash
docker swarm join --token TOKEN IP:PORT
```
Copy that command and run it on every node you want to join the swarm.

You can check the number of nodes with 
```bash
docker info
```

## 9 – Install portainer

  
Create first a volume for the portainer data
 ```bash
cd ~/
docker volume create portainer_data
 ```
Then run the follow to build and launch on port `9000`:

 ```bash
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
 ```

At this point, you just need to connect to:

[http://localhost:9000](http://localhost:9000/)

