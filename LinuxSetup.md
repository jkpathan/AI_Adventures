# Instructions on setup on Linux

If you have a personal linux machine, skip the first set of instructions on setting up an Azure GPU-enabled VM as your host machine. Because we will be using Pinokio which is based in a browser, you will need to get a graphical interface working on your machine

## Step 1: Get a Linux VM on Azure
1. Login to portal.azure.com
2. Create a new Virtual Machine and choose the following options:
    a. Choose Standard NV4as v4 (will give you 4 GPUs)
    b. Under 'Connect', setup native SSH with and have it generate SSH keys. Save the .pem file locally to use for connecting
    c. Under 'Networking', make sure to enable Public IP address so that you can connect to your VM remotely
3. Connect to your VM using the command below:
    `ssh -i [path to your .pem file] [ssh user name]@[Public IP Address]`

## Step 2: Get GUI running on the VM
1. Download / refresh package info
    `sudo apt-get update`
2. Install xfce4
    `sudo DEBIAN_FRONTEND=noninteractive apt-get -y install xfce4`
    `sudo apt install xfce4-session`
4. Install xrdp (remote desktop)
    `sudo apt-get -y install xrdp`
    `sudo systemctl enable xrdp`
    `sudo adduser xrdp ssl-cert`
5. Configure xrdp to use xfce4
    `echo xfce4-session >~/.xsession`
    `sudo service xrdp restart`
6. Set a password for your user since you can't use ssh cert to connect with xrdp. Replace the username below with the username you created in Step 1.
    `sudo passwd naziml`
7. Allow inbound port 3389 to your VM, by going to portal.azure.com and for the NSG of your VM adding an allow rule for inbound port 3389

## Step 2: Create and attach a 1000G disk to VM
API, models etc will take a lot of disk space, so you will need to mount external 1000G disk so that your OS disk doesn't fill up. Use this link to go set this up. https://learn.microsoft.com/en-us/azure/virtual-machines/linux/add-disk?tabs=ubuntu 

## Step 3: Install and configure Ollama
1. SSH into your VM
2. Install Ollama
    `curl -fsSL https://ollama.com/install.sh | sh`
3. Install llama2 models
    `ollama run llama2`
4. Exit out of ollama
    `\bye`

## Step 4: Install and configure Pinokio
1. Use remote desktop manager and use your VM's public IP address and the user name, password created in Step 2 to login to your VM1
2. Launch terminal and download Pinokio
    `curl -LO https://github.com/pinokiocomputer/pinokio/releases/download/1.2.0/Pinokio_1.2.0_amd64.deb`
3. Install the debian package with dependencies
    `sudo apt install ./Pinokio_1.2.0_amd64.deb`
4. Launch Pinokio browser
    `pinokio`
5. In the main settings page, you want to set the pinokio folder to a data drive that you mounted in Step 2 that has plenty of space, else you will run out of disk space pretty quickly. 
6. Install ComfyUI, Moondream and Lobechat from the 'Discover' section of the browser that pops up



    
