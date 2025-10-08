# Testing Grafana Generic OAuth with Keycloak

<br>

## Quick review

[image - upload from Sheets screenshot]

In our test, your browser will switch (redirect) between 2 hosts: Grafana on your local machine, and Keycloak on your EC2 host. We have a box around Keycloak because in our simple environment, it plays the roles of both the OpenIDConnect Provider (OP) and the Identity Provider (IDP). 

Grafana relies on Keycloak for authentication, so it is the Relying Party, or RP.


<br>

## Browser setup

I recommend having 2 browsers open while you test. In the browser you've been using, it's handy to keep Grafana and Keycloak logged in, since you are acting as an administrator of both of those services.

Using an incognito browser, you can play the role of an end user. So create a new incognito browser windown and in it, open up [Grafana on your local machine](http://localhost:3000). You should have a new session with Grafana in this browser, so you should be seeing the login screen, including the option to log in with Keycloak.

Click that login button. Your browser is redirected to Keycloak on the EC2 instance. There, you can log in as the user you created in the ```grafana-testing``` realm.

When that login is successful at Keycloak, your browser should be redirected back to Grafana, and you should be logged in. If anything didn't work, the next section can be helpful for debugging.


## Looking at application logs on both ends

Since you are running both of these services yourself, you have access to all of the application logs on both ends. Whether you're using Docker Desktop or another means of running Grafana as a container, you should be able to easily see your logs. When problems happen with OIDC, this is one of the first places to go: Grafana will try to tell you whatever it can in its logs about what happened during the OIDC flow.

In fact, it can be interesting to intentionally cause problems, like misconfigurations, or network connectivity problems, and see what Grafana has to say about it. Optional but fun.


## OIDC Flow

It's good to get to know [the OpenID Connect flow](https://infosec.mozilla.org/guidelines/iam/openid_connect.html#detailed-oidc-authentication-flow) a little better. Assuming you've successfully logged in and are in the Grafana home UI, the steps outlined in that flow will have happened. Knowing this flow might also be helpful in debugging OIDC setups down the road.






