# Node.js on DigitalOcean with Ansible
> Complete playbook to create and deploy a Node.js application on Digital Ocean.

## Intro
This project created to be the one and only destination to bootstrap new projects on [DigitalOcean](digitalocean.com) 
using [Ansible](https://www.ansible.com/).

I wanted to:
* Automate build and configuration of a droplet
* Automate deployments of my project

To start using this repo, you need a DigitalOcean account, 
a [DigitalOcean API key](https://cloud.digitalocean.com/settings/applications), a domain name and a
[Papertrail](https://papertrailapp.com) account along with its [API key](https://papertrailapp.com/account/profile).

Many thanks to https://github.com/yoshz/ansible-digitalocean cause I took a lot of his code.

## What's inside
This script will:

* Generate an SSH key on the local machine if it doesn't exist
* Create a droplet with the SSH key
* Add a domain and point it to the droplet
* Add swapfile to droplet
* Add a new root-user to the droplet and enable sudo
* Secure with ufw and restrict SSH to pub-key access only
* Setup Papertrail logging
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

## Internals
The main entrypoint is the [main.yml](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/main.yml) 
file. The playbook is divided in three plays one 
for the DigitalOcean configuration, one for the server initial configuration which runs as 
root user and the third for the specific project configuration.

The [tasks](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/tasks)
folder contains all tasks required by the playbook. Its file is named 
after its concern so it's easy to spot where something is happening.

You can place your project's specific files at the [files](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/files) 
folder.

Tweak the project using the [defaults/vars.yml](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/defaults/vars.yml) 
file. All of the values there are required, so make sure that are valid and cover your needs.

The main file you need to edit is the [tasks/project.yml](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/tasks/project.yml).
This file is called after the server is up and running. The example file will:

* Pull your repo (http://docs.ansible.com/ansible/git_module.html) only if it has changes to pull
* install local dependecies (http://docs.ansible.com/ansible/npm_module.html)
* install global dependecies like `webpack` and `pm2`
* build your repo using `webpack`
* start it using `pm2`

## Contribute
Please, do contribute by [opening an issue](https://github.com/kbariotis/ansible-nodejs-digitalocean/issue) 
or creating a Pull Request.

## License
[MIT](https://github.com/kbariotis/ansible-nodejs-digitalocean/blob/master/LICENSE.md)
