# Setting up an Arch Linux Server on DigitalOcean using `doctl` and `cloud-init`

## Introduction to DigitalOcean
DigitalOcean is a cloud computing service that provides access to remote servers. We will be using DigitalOceans services to create a virtual server or also known as a Droplet, that will run Arch Linux. We will also walk you through connecting the virtual server to your local machine using SSH Keys.

This Tutorial is aimmed at Term 2 CIT Students who have some knowledge of command-line functions but do not know how to create a cloud infrastructure. In the tutorial, we will cover generating SSH Keys, creating Droplets using `doctl` command-line tool, using cloud-init to configure your Droplet and adding a custom Arch Linux image.

## Overview
1. Creating a SSH Key on your device
2. Connecting your SSH Key to your DigtialOcean Account
3. Adding your Arch Linux Image onto DigitalOcean
4. Installing and Configuring `doctl`
5. Configure `cloud-init`
6. Create Droplet using `cloud-init` and `doctl`
7. Connecting to the Droplet via SSH 

