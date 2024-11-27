# Build a SCIM provisioning integration overview

## Build SCIM API service

### API endpoints

Must use `https://` prefix.

API list:

https://developer.okta.com/docs/api/openapi/okta-scim/guides/scim-20/

### Authentication

Okta supports authentication against SCIM APIs using any one of the following methods:

1. OAuth 2.0 Authorization Code grant flow
2. Basic Authentication
3. A custom HTTP header

For OAuth 2.0, after you successfully authorize Okta to use your SCIM API, your app's authorization server redirects you back to Okta, with either an authorization code or an access token.

If you plan to publish your integration to the OIN, Okta requires that all SCIM apps support the following redirect URIs (opens new window):

- `https://system-admin.okta.com/admin/app/cpc/{appName}/oauth/callback`
- `https://system-admin.okta-emea.com/admin/app/cpc/{appName}/oauth/callback`
- `https://system-admin.oktapreview.com/admin/app/cpc/{appName}/oauth/callback`
- `https://system-admin.trexcloud.com/admin/app/cpc/{appName}/oauth/callback`
- `http://system-admin.okta1.com:1802/admin/app/cpc/{appName}/oauth/callback`

### Base URL

If you're implementing a new SCIM API, Okta suggests using `/scim/v2/` as your base URL.

For example: `https://example.com/scim/v2/`

### User schema

#### Basic user schema

Okta requires that your SCIM implementation stores the following four user attributes:

- User ID: `userName`
- First Name: `name.givenName`
- Last Name: `name.familyName`
- Email: `emails`

If your integration supports more than the four base user attributes, you can add them to your SCIM API. Sometimes, you might need to configure Okta to map non-standard user attributes to the user profile for your app:

https://help.okta.com/en-us/okta_help.htm#cshid=ext_Directory_Profile_Editor_Tasks

#### Unique ID

In addition to the basic user schema attributes, your SCIM API must also specify a unique identifier for each SCIM resource, including users and groups. Okta uses this unique identifier to link the Okta profile with the SCIM resource.

SCIM specification asserts that the `id` attribute is used to identify resources. The unique identifier has the following properties and behaviors:

- Assigned a value by the Service Provider (your app) for each SCIM resource
- Always issued by the Service Provider (your app) and not specified by the client (Okta)
- Must be included in every representation of the resource
- Can’t be empty
- Must be unique across the SCIM Service Provider's entire set of resources
- Can't be reassigned
- Must be a stable identifier, meaning that it doesn't change when the same resource is returned in subsequent API requests
- Must be case-sensitive and read-only
- Can't be hidden from the API request
- As a best practice, generate a global unique identifier (GUID) for this value.

Note: You can't use the string bulkId within any unique identifier value. It’s a reserved keyword.

#### Active resources

Okta user management requires that your SCIM API supports an `active` attribute for each user resource that can be set to `true` or `false` to denote a resource as active or inactive.

### An example

https://github.com/imulab/go-scim

## Configure Okta to use your SCIM API service

Create and test a private SCIM integration from your Okta org to your SCIM API service.

To test and submit your SCIM integration to the Okta Integration Network (OIN) for public use, see Submit an integration with the OIN Wizard.

https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_scim.htm

You must configure your app integration to verify signed SAML assertions for SSO and trust **Okta as the Identity Provider**.

## Test your Okta SCIM integration

https://developer.okta.com/docs/guides/scim-provisioning-integration-test/main/
