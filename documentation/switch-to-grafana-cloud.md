# Switching to Grafana Cloud

<br>

## What's the same, what's different

There's not a large difference between setting up Cloud Grafana for OIDC, and setting up a local instance as you've now done.

These are the primary differences:

- You'll want to make a second client in Keycloak, with its own ID, its own secret, and a few other settings that are specific to your Cloud Grafana instance.

- You'll set up Generic OAuth in Cloud Grafana almost exactly as you did with Grafana on your local machine; there are just a few fields that are slightly different.

- Assuming you followed the recommended networking setup on EC2, you will need to make some changes to your inbound firewall rules, so that Cloud Grafana can make the OIDC API calls to Keycloak, after a successful login (or at other times, like during token refresh, but the current version of this repo won't go into that topic in detail.)

This next section will talk first about the networking changes.


## Networking changes

In the [Keycloak on EC2 instructions](../documentation/keycloak-EC2-setup.md), we said: 

```for now just enable 22 (SSH) and 8443 (Custom TCP), both allowing your IP address ("My IP" in the AWS UI) and no other IP addresses.```

So with Grafana on your local machine, the browser started at [http://localhost:3000](http://localhost:3000), redirected to Keycloak on your EC2 VM, and then redirected back to localhost. Subsequently, Grafana (the program itself) on localhost is able to make subsequent API calls to Keycloak on the EC2 VM. And your Security group had all the ports you needed: 22 (for SSH), and 8443 (for Keycloak), and both were limited to only be visible from your locataion ("My IP").

For Cloud Grafana to connect to Keycloak, you have a couple of options.

<br>

### Option 1:

First, you could just change the inbound rule for port 8443 in your Security Group, so that it's open to the entire internet. If you are comfortable doing this, it is the easiest step to take. Obviously it's also the riskiest; but your Keycloak is only listening on a secure port (with a self-signed certificate), and you hopefully have not published any Keycloak login credentials in insecure places.

## Use Option 1 at your own risk! ##

If you choose to follow that path: it's entirely your decision; this repo is for educational purposes only, and takes no responsibility for actions you take with your networking and security.


<br><br>

### Option 2:

The second path is to change your inbound rule for 8443, such that your location can still see it, but also hosted Grafana can also see it. 

There is [a page here discussing Grafana's IPs that you can add to your allowlist](https://grafana.com/docs/grafana-cloud/security-and-account-management/allow-list/). For our case, you should look specifically at [the section on Hosted Grafana](https://grafana.com/docs/grafana-cloud/security-and-account-management/allow-list/#hosted-grafana). 

The complete IP list is not small; however if you know which cluster your Cloud Grafana instance is running in, you can narrow this list down. One way to do this is with ```dig```. Here are a couple of examples:

- if you are running in US-central: ```dig +short src-ips.prod-us-central-0.hosted-grafana.grafana.net```
- if you are running in Tokyo: ```dig +short src-ips.prod-ap-northeast-0.hosted-grafana.grafana.net```

Depending on your region, you may have a relatively large number of IPs to add to your allowlist. You might want to check out using "Managed Prefix Lists" on AWS, where you can set up a long list of IPs, and then add them all in one step in your Security Group.



## Making a second client in Keycloak

It's nice to keep your first client available for testing a local instance of Grafana, but to also have a second client for testing with Cloud Grafana. So you could review [the "Create a client (manual method)" instructions here](../documentation/keycloak-initial-configuration.md), and use slightly different values this time:

- Client ID: call this one "confidential-client-grafana-cloud"

- "Capability config": follow the same instructions as you did for the first client

- "Login settings": instead of using the localhost examples (http://localhost:3000, etc.), replace "localhost:3000" with the domain name of your Cloud Grafana instance. Also, switch all your protocols from ```http:``` to ```https:```, since Cloud Grafana runs only with secure protocols.

As mentioned on that page, you can also start using a JSON file supplied with this repo, for creating the Cloud Grafana client. These instructions won't go into more detail on that for now, but it's pretty easy to figure out.


## Setting up Generic OAuth in Cloud Grafana

This is also almost the identical process as you followed with Grafana on your local machine. The only difference is that you need to use the new Client ID you set up ("confidential-client-grafana-cloud" above), and go get the Client Secret from this new client in Keycloak - go to the new client, look for Credentials, and get the Client Secret, and paste it into the Grafana UI.



<br><br>

## Next steps

We're ready test [logging into Cloud Grafana with Keycloak](../documentation/test-2.md).


