# Ansible DigitalOcean Node.js
> Complete script to create a Node.js droplet on Digital Ocean.

## Intro
This project created to be the one and only destination to bootstrap new projects using on DigitalOcean.

I wanted to:
* Automate build and configuration of a droplet
* Automate deployments of my project

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
You need Ansible to start with this project. So go get it!

* Clone this repo
* Edit `defaults/vars.yml`
* Edit `tasks/project.yml` and add your own rules

Make sure to add the `launch` and `deploy` tags (or any other you need) to your own tasks.

Then run:

`ansible-playbook main.yml --tags=launch` to launch the server
`ansible-playbook main.yml --tags=deploy` to deploy the server
`ansible-playbook main.yml --tags=certificate` to manually request a new certificate

## Commands

## License
