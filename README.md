# Setting up an Arch Linux Server on DigitalOcean using `doctl` and `cloud-init`

## Introduction to DigitalOcean
DigitalOcean is a cloud computing service that provides access to remote servers. We will be using DigitalOceans services to create a virtual server or also known as a Droplet, that will run Arch Linux. We will also walk you through connecting the virtual server to your local machine using SSH Keys.

This Tutorial is aimed at Term 2 CIT Students who have some knowledge of command-line functions but do not know how to create a cloud infrastructure. In the tutorial, we will cover generating SSH Keys, creating Droplets using `doctl` command-line tool, using cloud-init to configure your Droplet and adding a custom Arch Linux image.

## Overview
1. [Creating a SSH Key on your device](#creating-a-ssh-key-on-your-device)
2. [Installing and Configuring `doctl`](#installing-and-configuring-doctl)
3. [Configuring `cloud-init`](#configuring-cloud-init)
4. [Creating a Droplet using `doctl`](#creating-a-droplet-using-doctl)
5. [Connecting to the Droplet via SSH](#)

# Instructions
## Creating a SSH Key on your device
We need to create a SSH Key to securely connect to your remote server. SSH is an encrypted connection method that provides us with more security than password-based authentication as the private SSH Key is stored on your device, ensuring only you can access the server.

**Step 1:** Open the **Terminal** on your device

**Step 2:** Type this code into your terminal
```
ssh-keygen -t ed25519 -f C:\Users\your-username\.ssh\key-name -C "your-email-address"
```

![SSH Key Code](Images/creating_sshkey.jpg)

> **Note:** Change `your-username` with the current user in the terminal (in the picture above it would be kimsu), `key-name` with your desired Key name, and `your-email-address` with your desired email address

![SSH Key](Images/sshkey.jpg)

> Your key should look something like this

**Step 3:** Check that both your private key and public are in the .ssh directory by typing `cd ~/.ssh` and then `ls`

![Keys in .ssh folder](Images/checkingbothkeys.jpg)

> If both keys are present than you have successfully created the keys

**Congratulations you have successfully created a SSH Key Pair!**

## Installing and Configuring `doctl`

### What is `doctl`?
`doctl` is the DigitalOcean CLI used for interacting with your DigitalOcean account and managing resources. For this tutorial we will be using `doctl` to help us create our Droplet from a existing Droplet
> We will be splitting this step into 4 simple parts:
> 1. Installing `doctl` on Arch Linux
> 2. Creating an API token
> 3. Using the API token to grant account access to `doctl`
> 4. Validate that `doctl` is working

### Installing `doctl` on Arch Linux

In your **Terminal** Run the code below 
```
sudo pacman -S doctl
```

**You have successfully installed `doctl` on Arch Linux!**

### Creating an API token
To use the API, we need to create a personal access token and we will be using the token to authenticate and connect to the API. We will be creating the API and token in the DigitalOcean website
>**Warning:** Keep your token secret as they act like passwords.  

**Step 1:** Click on **API** on the left-handed side of the menu and click **Generate New Token**

**Step 2:** On the **Create a New Personal Access Token** fill out the following:
> - Token Name
> - Expiration
> - Scopes - Based on your team role

![Filling out your personal access token](Images/personaltokengeneration.jpg)

**Step 3:** Click **Generate Token**
> **Warning:** After generating the token, it will show you your personal token code. This code will only appear once, so make sure to save your token somewhere safe

**Congratulations, you have successfully created a New Personal Access Token through the API!**

### Using the API token to grant account access to `doctl`

**Step 1:** In your **Terminal** type the the code below
```
doctl auth init --context <NAME> 
```
> **Note:** Change the *NAME* to a name of your choice

> This code will create an authentication context which we need to connect to through our API Token. (*)

**Step 3:** The terminal will prompt you for your Personal token code. Paste your token code into the terminal and press **Enter**

**Congratulations, you have granted account access to `doctl` using your API token!**

### Validate that `doctl` is working
This step will check if you have configured and installed `doctl` correctly

> Make sure you are switched into your account. If you are not in the account you can switch into it by typing `doctl auth switch --context <account-name>` and changing *account-name* to the name of the account

Type this command below into your terminal
```
doctl account get
```
![`doctl` code and result](Images/validatingaccount.jpg)

> Your output should look something like this

**Congratulations, your `doctl` is working!**

## Configuring `cloud-init`
`cloud-init` is a tool that will help us configure a new system using a YAML configuration file. It will help automate the following tasks
- Create a new user
- Install initial packages
- Add the public SSH key to the new user’s `authorized_keys`
- Disable root access via SSH

**Step 1:** Install Neovim with the code below
```
sudo pacman -S neovim
```

**Step 2:** Create a YAML file using Neovim with the code below

```
nvim <cloud-file-name>.yaml
```
> **Note:** Change `<cloud-file-name>` to your desired name 

**Step 3:** Add the contents below into your YAML file

```
users:
  - name: <name>
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519 <public key> <email>
disable_root: true
packages:
  - nginx
  - neovim
  - git 
  - htop
```

> **Note:** Replace `<name>` with a username, `<public key>` with your public key that you made earlier, and `<email>` with the same email you created your key with

**Congratulations, you have successfully configured your `cloud-init` YAML file!**

## Creating a Droplet using `doctl`

## Connecting to the Droplet via SSH