# grafana-keycloak
enablement material for SSO setup between Grafana and Keycloak

## Summary

This project helps you quickly set up Grafana and Keycloak, and then easily provision them so that 
you can see a working environment.

Your Keycloak instance will run on an AWS EC2 VM, while Grafana will run first on localhost, and then optionally in Grafana Cloud.

With Grafana on localhost and Keycloak on EC2, you will have access to both sets of logs, for visibility into issues such as misconfigurations.

The section on Keycloak will include instructions for helping you be sure that your Keycloak instance is only accessible from the IP addresses you select (your home, Grafana Cloud, etc.)

This is a first draft of these notes and therefore will be somewhat terse, assuming you know are bringing some prerequisite knowledge. Later versions of this may expand to add more enablement content on the prerequisites but no promises are made as of now.

Wherever possible, automation solutions will be used, for example Docker Compose and Terraform.

<br><br>
## EC2 setup

You will need one EC2 instance. Here is what I have tested with:

- OS: Ubuntu Server 22.04 LTS
- Instance type: t2.small
- Storage: 16 GiB (could probably be smaller but haven't tested)

I will come back to the topic of inbound rules; for now just enable 22 (SSH) and 8443 (Custom TCP) for "My IP Address" and no others.

Boot the VM and log into it, then I recommend running ```sudo apt-get update``` upon first arrival.


<br><br>
## Docker and Docker Compose on the EC2 VM

For Docker, I recommend following [the instructions on this page](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04), until you have completed and tested Step 2 (running docker without Sudo).

For Docker Compose, I recommend following [the instructions on this page](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04), until you can successfully run "docker compose version".


<br><br>
## Clone this repo onto the VM

I recommend doing this in the ubuntu user's home directory, as follows:
```git clone https://github.com/danstadler-pdx/grafana-keycloak.git```



