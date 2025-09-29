# Grafana setup on a local machine

<br><br>
## Docker and Docker Compose

This project was built on a MacBook Pro, running MacOS Sequoia 15.6.1. 

If you are using a similar machine, I recommend using Docker Desktop so that you have Docker and Docker Componse installed and ready.



<br><br>
## Clone this repo onto the local machine

I recommend starting in a subdirectory of your own home directory. For example: 

```[your home directory]/code```

In that directory, run this command:

```git clone https://github.com/danstadler-pdx/grafana-keycloak.git```

Then cd into the directory "grafana-keycloak".



<br><br>

## Configure Grafana

To do this, cd into the directory "docker", then cd into the directory "grafana".

In that folder is a Docker Compose file. Edit the file, providing a password for the env var ```GF_SECURITY_ADMIN_PASSWORD```. (Feel free to also change the admin username if you want.)

Save the file.


<br><br>

## Run Grafana

Still in the docker/grafana directory, run: 

```docker compose up```

or

```docker compose up -d```

If running with the ```-d``` arg, you can tail the container logs using 

```docker logs grafana -f```


You should be able to log into the grafana UI at [this url](http://localhost:3000).


<br><br>

## Next steps

Next we'll [set up a realm, user, and client](../documentation/keycloak-initial-configuration.md) in Keycloak.

