# Logging into Cloud Grafana with Keycloak


## Test the new client

This test is basically [the same test you ran with local Grafana](../documentation/test-1.md); the one key difference is that in your incognito browser window, you need to start with URL of your Cloud Grafana instance. In this incognito window, there should be no current session with that Grafana instance, so you should see the login page and the button to log in with your Keycloak instance.

If you are still logged in to Keycloak itself, login should be a one-click operation for you. You can also try logging out of Keycloak itself, using the instructions in the first test, where you opened a second incognito tab to your end-user's Account Management page in Keycloak.


<br><br>

# Congratulations!

At this point, you now have run Keycloak as an OP/IDP, with Grafana as the Relying Party, and successfully implemented Generic OAuth with OpenIDConnect.