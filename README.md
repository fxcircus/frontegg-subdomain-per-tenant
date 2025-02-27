# Frontegg - Subdomain per Tenant

This repository demonstrates how to set up a dedicated subdomain for each of your clients using Frontegg. For example, our parent domain can be `royhamelekh.com` and then we can add three different tenants\accounts in Frontegg, each with its own dedicated subdomain:

- The first client 'Client1' will be accessible from - [https://client1.royhamelekh.com/](https://client1.royhamelekh.com/)
- 'Client2' will be accessible from [https://client2.royhamelekh.com/](https://client2.royhamelekh.com/)
- And 'Client3' will be accessible from [https://client3.royhamelekh.com/](https://client3.royhamelekh.com/)
(You can add as many accounts\tenants as you'd like in the future)

We use [Embedded login with Frontegg](https://docs.frontegg.com/docs/react-embedded-login-guide) and the [Customization per tenant feature](https://docs.frontegg.com/docs/custom-login-box).

## Part 1: Frontegg Account Setup

1. **Enable Embedded Login Mode:**
   - Navigate to `Frontegg Portal ➜ [ENVIRONMENT] ➜ Authentication ➜ Login Method`.
   - Select `Embedded login` mode.
   - ![embedded_mode](./assets/01_embedded.png)

2. **Set Up a Custom Domain:**
   - Follow the instructions [here](https://docs.frontegg.com/docs/custom-domains).
   - ![custom_domain](./assets/02_domain.png)

3. **Update Application URLs:**
   - Open your application settings and change the Login URL and App URL to use the custom domain.
   - ![application_urls](./assets/03_app_url.png)

4. **Create Dedicated Accounts for each Client:**
   - Go to `Backoffice ➜ Accounts` and create an account for each client.
   - ![accounts_in_backoffice](./assets/04_accounts.png)

5. **Enable customization-per-tenant:**
   - Enable the [customization per tenant feature](https://docs.frontegg.com/docs/custom-login-box) on each of the accounts.
   - Navigate to `Frontegg Portal ➜ [ENVIRONMENT] ➜ Backoffice ➜ Accounts ➜ [CLIENT_ACCOUNT] ➜ Settings ➜ Custom login` to tie the subdomain to the account.
   - Toggle the `Live` button to apply the change and make this account accessible.
   - *Note that you can also customize the login box styling and authentication methods when using this feature (But you don't have to)
   - ![custom_login](./assets/05_custom_login.png)

## Part 2: Running this code
Clone this repo.
Open `src\index.js` and add the values from your environment:
- `baseUrl`: Login URL from `Frontegg Portal ➜ [ENVIRONMENT] ➜ Applications`:
- `appId`: Application ID from `Frontegg Portal ➜ [ENVIRONMENT] ➜ Applications`:
- `clientId`: Client ID and API Key from `Frontegg Portal ➜ [ENVIRONMENT] ➜ Env Settings page`.
```
const contextOptions = {
  baseUrl: 'https://[YOUR_DOMAIN].com',
  clientId: '[YOUR-CLIENT-ID]',
  appId: '[YOUR-APP-ID]'
};
```
Here is the relevant code for the `tenantResolver` function, it grabs the subdomain and redirects the user to the relevant account\tenant:
```
tenantResolver: () => {
  const domainParts = window.location.host.split('.');
  // Check if the host is accessed with a subdomain
  if (domainParts.length > 2) {
    const organization = domainParts[0]; // Assuming 'client1' is the tenant identifier i.e 'client1.yourdomain.com'
    return { tenant: organization };
   } else {
    return { tenant: undefined }; // Default tenant or behavior for accessing the root domain (this is the default login box you see in the builder)
}
```


Host the project on a development platform like Netlify in order to use your domains. You can also use a service like Ngrok to work locally.

#### Important notes when using the customization per tenant feature
- User signup is not available at the moment for accounts with this feature. Only users that are invited to the accounts will be able to login (except if using SSO).
- This is not supported for our NextJS SDK (Currently NextJS is supported only with query params)
