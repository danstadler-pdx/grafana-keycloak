# Keycloak setup on EC2

<br><br>
## Create an EC2 instance

You will need one EC2 instance. Here is what I have tested with:

- OS: Ubuntu Server 22.04 LTS
- Instance type: t2.small
- Storage: 16 GiB (could probably be smaller but haven't tested)

We will come back to the topic of Security Groups and inbound rules; for now just enable 22 (SSH) and 8443 (Custom TCP), both allowing your IP address ("My IP" in the AWS UI) and no other IP addresses.

Boot the VM, use the AWS instructions to SSH onto it, and I recommend running ```sudo apt-get update``` upon first arrival.


<br><br>
## Recommended: use an Elastic IP

It can be helpful to allocate a fixed IP address, and associate it with this EC2 VM. If you don't, and you shut down the VM, you will have a new IP address upon next boot, and then have to change your OpenID configuration to reflect this. Using a static IP address helps eliminate this problem.

If you take this step, you will need to reboot the VM once, so that the static IP becomes associated to it.


<br><br>
## Install Docker and Docker Compose on the EC2 VM

For Docker, I recommend following [the instructions on this page](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04), until you have completed and tested Step 2 (running docker without Sudo).

For Docker Compose, I recommend following [the instructions on this page](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04), until you can successfully run "docker compose version".


<br><br>
## Clone this repo onto the VM

I recommend doing this in the ubuntu user's home directory, as follows:

```git clone https://github.com/danstadler-pdx/grafana-keycloak.git```

Then cd into the directory "grafana-keycloak".


<br><br>
## Build the keycloak docker image, including the self-signed certificate.

First cd into the directory "docker", then cd into the directory "keycloak".

Edit the Dockerfile in this directory. You need to modify the content of the line which starts with:

```RUN keytool -genkeypair```

When you first check out this repo, that line should have a section in it like this:

```-ext "SAN:c=DNS:localhost,IP:127.0.0.1"```

You need to add the IP address of your EC2 VM. This can either be the ephemeral address it gets upon boot, or the static IP you've allocated and associated to the VM.

Based on that choice, the content for ```-ext``` can look like either of the 2 options below (*use your own actual values taken from the EC2 UI:*)

<br>

*(if using an ephemeral IP: see the "DNS:" entry at the end of the list):*

```-ext "SAN:c=DNS:localhost,IP:127.0.0.1,DNS:ec2-23-19-184-210.us-west-1.compute.amazonaws.com"``` 

*(if using a static IP: see the second "IP" entry, at the end of the list):*

```-ext "SAN:c=DNS:localhost,IP:127.0.0.1,IP:59.62.136.115"```

<br>

Make that change and save the Dockerfile. Then, in the same directory as the Dockerfile, run the following command:

``` docker build . -t mykeycloak ```

When the build is done, cd back up one level, i.e. to the root of the cloned project.

<br>

Before doing the next step: at the root of the cloned project, run this command:

```mkdir keycloak-data```

<br>

> TODO: this next part is temporary, until I put back together the Compose file for Keycloak, and add instructions for managing credentials better than described here.

Create a username and strong password that you will use for the admin login to Keycloak. Store them safely, not in this code repo.

Then, make a copy (i.e. in a textfile on your laptop) of this next snippet, insert your username and password where marked, and then run the whole command, from the root folder of the cloned project:

> You can choose to remove the ```-d``` argument and have the container tied to the command line. This will dump out all the logs for you, but it also means the container will stop if your terminal session dies. It's OK for early runs, but I recommend using -d as you get more comfortable.

```
docker run \
-d \
--name mykeycloak \
-p 8443:8443 \
-p 9000:9000 \
-e KC_BOOTSTRAP_ADMIN_USERNAME=[your admin username] \
-e KC_BOOTSTRAP_ADMIN_PASSWORD=[your strong admin password] \
-v /home/ubuntu/keycloak/keycloak-data/:/opt/keycloak/data/ \
mykeycloak \
start-dev
```

Once this is completed, you should have a running Docker container called "mykeycloak". If you are using the ```-d``` arg and you'd like to see the logs, you can run 

```docker logs mykeycloak -f``` 

and you will be tailing the container logs. (It can also be helpful to have a second terminal window open on the same VM, and run the ```docker logs``` command there, while using the first terminal for other work on the VM.)


<br><br>

## Logging into Keycloak

Using the DNS or Elastic IP of the VM, open up the webpage: 

```https://[your VM's address]:8443/```

The broswer should immediately present you with a warning that the site is unsafe. That's because your Docker build step included the creation of a self-signed certificate. Since you built it, and since it's assumed that only your home or office IP address was added to the EC2 Security Group Allow List, you should be safe telling the browser to accept and use this certificate.

> If you have any questions or concerns about this, please speak with your management or IT/Security team before accepting and using this certificate.

Once you get to the login page, enter the username and password you created for Keycloak. After logging in, you should be seeing the "Master Realm" of Keycloak.

<br><br>

## Next steps

With practice, you can get used to quickly doing basic things like creating your own Realm, adding one or more Users, and adding one or more Clients.

We will be covering those in the docs section right after the Grafana setup. However if you'd like to go have a preview of using Keycloak, I recommend watching [this video on youtube](https://www.youtube.com/watch?v=fvxQ8bW0vO8), or other enablement material of your choosing.

If you want to move on to installing Grafana on your local machine, please proceed to [this page](../documentation/grafana-local-machine-setup.md).