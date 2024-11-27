# Okta Integration With SCIM Solution

## OIDC or SAML?

### SCIM + OIDC

Need to create two separate app integrations.

From doc: https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_scim.htm

> Using the App Integration Wizard, create a custom SSO integration using either SAML or SWA.
>
> Adding SCIM provisioning to an OpenID Connect (OIDC) integration isn't currently supported.

From support (Nov 5, 2024): https://support.okta.com/help/s/article/configure-scim-for-a-custom-oidc-app?language=en_US

> Enabling SCIM provisioning with a custom OpenID Connect (OIDC) integration is not currently supported.
>
> If Admins have confirmed that the provider supports both features, they can create two separate app integrations: one for the OIDC configuration and the other for the SCIM configuration. The SSO portion of the provisioning-only app should not need to be functional as long as both integrations are configured appropriately.

From forum: https://devforum.okta.com/t/why-is-there-no-scim-option-for-oidc-apps/24737

> There are apps that support both OIDC and SCIM in the Okta Integration Network (OIN). It looks like they are able to merge the integrations into one app.
>
> If you’re asking for custom OIDC apps, they currently do not support SCIM and would require a separate app.

(The app mentioned above is some app like Zscaler: https://help.zscaler.com/zidentity/configuring-okta-external-idp)

From Auth0: https://auth0.com/docs/authenticate/protocols/scim/inbound-scim-for-okta-workforce-connections

> This integration will require two applications to be registered in Okta Workforce: the OpenID Connect integration and the SCIM integration. The same users and groups must be assigned to both.

Steps:

1. Create first App for OIDC SSO:

- https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_oidc.htm

- https://auth0.com/docs/authenticate/identity-providers/enterprise-identity-providers/okta

2. Create second App for SCIM:

- https://auth0.com/docs/authenticate/protocols/scim/inbound-scim-for-okta-workforce-connections

Tests:

1. Add user: the user is created in Auth0 and can access console ui.
2. Add a group: users in the group are created in Auth0 and can access console ui.
3. Add user to a assigned group: the user is created in Auth0 and can access console ui.
4. Remove user or remove users from group:
   1. Auth0 users are not deleted.
   2. Users can still access console ui until they log out and try to log in again. Then the login will fail.

### SCIM + SAML

**Due the limitation and price of Auth0 quota, we currently don't support this method.**

[Auth0: Inbound SCIM for Okta Workforce SAML Connections](https://auth0.com/docs/authenticate/protocols/scim/inbound-scim-for-okta-workforce-saml-connections)

Example video(Zscaler): https://play.vidyard.com/DmYrUHPJDeH9BXy5AQMTZt

Steps: https://community.auth0.com/t/saml-setup-for-okta-as-idp-and-auth0-as-sp/91164

#### Okta Side Setup:

1. Sign in to the Okta Developer Console.

2. Go to Create App Integration and choose SAML 2.0 from the options.

3. In Single sign on URL, enter the Auth0 tenant’s login callback URL: https://YOUR_DOMAIN/login/callback?connection=YOUR_CONNECTION_NAME
   i.e. `https://suger-dev.us.auth0.com/login/callback?connection=okta-saml-test`

4. Set the Audience URI (SP Entity ID): `urn:auth0:YOUR_TENANT{just tenant name}:YOUR_CONNECTION_NAME`
   i.e. `urn:auth0:suger-dev:okta-saml-test`

5. Click next. Here, click View SAML Setup Instructions, where you will find the Identity Provider Single Sign-On URL, which should look something like: https://OKTA_TENANT_DOMAIN.okta.com/app/…/…/sso/saml and X.509 Certificate, which needs to be downloaded for later use when it will need to be uploaded into the Auth0 SAML connection setup.

6. Click Finish.

#### Auth0 Side Setup

1. Login to the Auth0 Dashboard. Navigate Authentication > Enterprise.

2. Click on the + sign next to the SAML connection.

3. Give the connection the same name used previously to setup the Okta Application for Single sign on URL and URI.

4. Set the Sign-in URL, which should look something like:https://OKTA_TENANT_DOMAIN.okta.com/app/…/…/sso/saml that can be found in the OKTA View SAML Setup Instructions.

5. Upload the X.509 Certificate, which was downloaded from the OKTA View SAML Setup Instructions screen described above.

6. Click Save Changes at the bottom of the screen.

7. On the Applications tab, toggle on to create an association between the Application and the desired connection.

8. Enable IdP-initiated SSO

9. SCIM setup: https://auth0.com/docs/authenticate/protocols/scim/inbound-scim-for-okta-workforce-saml-connections#configure-scim-settings-in-auth0

10. The setup is now complete: test by navigating to Dashboard > Authentication > Enterprise > SAML < connection name> , three dots on the right, and Try the connection link.
