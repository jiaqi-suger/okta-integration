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

### SCIM + SAML

[Auth0: Inbound SCIM for Okta Workforce SAML Connections](https://auth0.com/docs/authenticate/protocols/scim/inbound-scim-for-okta-workforce-saml-connections)

Example video(Zscaler): https://play.vidyard.com/DmYrUHPJDeH9BXy5AQMTZt

Steps: https://community.auth0.com/t/saml-setup-for-okta-as-idp-and-auth0-as-sp/91164
