# Node.js on DigitalOcean with Ansible
> Complete script to create a Node.js droplet on Digital Ocean.

<p align="center">
  <img src="https://raw.githubusercontent.com/kbariotis/ansible-nodejs-digitalocean/master/logo.jpg?raw=true" alt="Ansible Node.js DigitalOcean"/>
</p>

## Intro
This project created to be the one and only destination to bootstrap new projects on [DigitalOcean](digitalocean.com) 
using [Ansible](https://www.ansible.com/).

I wanted to:
* Automate build and configuration of a droplet
* Automate deployments of my project

All you need to start using this repo is a DigitalOcean account, 
an [API key](https://cloud.digitalocean.com/settings/applications) and 
a domain name as well.

Many thanks to https://github.com/yoshz/ansible-digitalocean cause I took a lot of his code.

## What's inside
This script will:

* Generate an SSH key on the local machine if it doesn't exist
* Create a droplet with the SSH key
* Add a domain and point it to the droplet
* Add swapfile to droplet
* Add a new root-user to the droplet and enable sudo
* Secure with ufw and restrict SSH to pub-key access only
* Add nginx as a reverse proxy
* Request a certificate from LetsEncrypt

## How to use it
You need Ansible to start with this project. [So go get it](http://docs.ansible.com/ansible/intro_getting_started.html)!

* Clone this repo
* Edit `defaults/vars.yml`
* Edit `tasks/project.yml` and add your own rules

Make sure to add the `launch` and `deploy` tags (or any other you need) to your own tasks.

Then run:

`ansible-playbook main.yml --tags=launch` to launch the server

## Commands

`ansible-playbook main.yml` or `ansible-playbook main.yml --tags=launch`

for the initial setup. Make sure that you've obtained your domain and already pointed 
to [DigitalOcean's nameservers](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars). 
It's better to wait a few hours for the changes to be propagated.

`ansible-playbook main.yml --tags=deploy` 

to deploy your app. Make sure to add the apropriate tags in 
[tasks/project.yml] (https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/tasks/project.yml) 
when adding your custom tasks.

`ansible-playbook main.yml --tags=certificate` 

to renew the certificate.

## [License](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/LICENSE.md)
