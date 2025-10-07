# Grafana Generic OAuth initial configuration


<br><br>
## Configure Generic OAuth

This step is pretty quick. In Grafana's left-nav, look near the bottom for Administration / Authentication, or, just go to (this link)[http://localhost:3000/admin/authentication]

<br>

Once on that page, push the large button for Generic OAuth. (These instructions assume you don't already have this configured on this Grafana instance.)

<br>

You should probably review [this page of the Grafana Documentation](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/generic-oauth/)

But in short, here are the values you will need to fill in, while using Grafana on your local machine; for any fields not listed below, just leave the default values in place.

### Display name: 
Choose a name that you will want to see for Keycloak, on Grafana's login page

### Client ID:
```confidential-client-localhost-grafana```  (this has to match the ID of the Client you created in Keycloak)

### Client secret:
You can get this from the Client page in Keycloak. From the Client list page, click on ```confidential-client-localhost-grafana```
, and in the resulting page, go to the ```Credentials``` tab. Copy the Client Secret from that page, and use it for this field in Grafana.

### Scopes:
You need 3 scopes: ```email```, ```profile```, and ```openid```. Type them one at a time, and use "hit enter to add" to add each one.

### OpenID Connect Discovery URL:
You won't use this, rather you'll use the next 3 fields, but just noting that this is worth exploring as an alternative to directly adding the next 3.

### OpenIDConnect URLs (three of them):
The next 3 all depend on whether you used a static IP (Elastic IP), or are just using an ephemeral VM IP address. These examples use a static IP,
you will need to change the IP to be your actual IP address associated to your Keycloak VM.

Also note that we're assuming your realm is called "grafana-testing", as described earlier in this project.


#### Auth URL: 
```https://123.234.345.456:8443/realms/grafana-testing/protocol/openid-connect/auth```

#### Token URL: 
```https://123.234.345.456:8443/realms/grafana-testing/protocol/openid-connect/token```

#### API URL: 
```https://123.234.345.456:8443/realms/grafana-testing/protocol/openid-connect/userinfo```


### Remaining fields:
For the remainder of this testing, you can use the following settings:

#### Allow sign-up
```enabled```

#### Auto login
```disabled```


<br>
Save the form; you will be returned to the list of Auth providers (free free to re-open Generic OAuth if you want to view your current config).



<br><br>

## Next steps

We're ready to start [testing Grafana Generic OAuth setup with Keycloak](../documentation/test-1.md).

