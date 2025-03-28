# Implementing Authentication Systems:
FIM allows different organizations to use federations for SSO.
## Implementing SSO on the Internet
Imagine you want to transfer money from Bank A to Bank B. You could give Bank B your credentails to Bank A and have them transfer the money. Sound scary? You bet. You should never be required to give your credentials to any third party. Solutions such as SAML, OAuth, OpenID, and OIDC help solve this problem. They share authentication, authorization, or profile information about a user, and some solutions share all three.
- **XML:** Databases from multiple vendors can import and export data to and from an XML format, making XML a common language used to exchange information if companies agree on what schema to use. Many cloud-based providers use XML-based languages to share information for authentication and authorization. They don’t use XML as it is but instead use other languages based on XML.
  - SAML: an open XML-based standard commonly used to **exchange authentication and authorization (AA) information** between federated organizations. It **provides SSO capabilities** for browser access. The Organization for the Advancement of Structured Information Standards (OASIS), a nonprofit consortium that encourages open standards development, adopted SAML 2.0 (a convergence of SAML 1.1, the Liberty Alliance Identity Federation Framework, ID-FF, 1.2, and Shibboleth 1.3) as an OASIS standard in 2005 and has maintained it since then. (www.oasis-open.org/standards) site has more details on SAML 2.0.
  - **Example:** imagine Sally is accessing her investment account at ucanbeamillionaire.com that requires her to log on to access her acocunt, and the site uses SAML:
    - Principal or User Agent: think of Sally as the principal who’s trying to access her investment account at ucanbeamillionaire.com
    - Service Provider (SP): ucanbeamillionaire.com site is providing the service
    - Identity Provider (IdP): This is a third party that holds the user authentication and authorization information. It can send three types of XML messages known as assertions:
      - Authentication Assertion: provides proof that the user agent provided the proper credentials, identifies the identification method, and identifies the times the user agent logged on.
      - Authorization Assertion: indicates whether the user agent is authorized to access the requested service. If the message indicates access is denied, it indicates why.
      - Attribute Assertion: can be any information about the user agent.
- **OAuth (OAuth 2.0 implying open authorization):** an authorization framework described in RFC 6749 and maintained by the Internet Engineering Task Force (IETF). Many companies on the internet use it to share account information with third-party websites.
  - **Example:** imagine you have a Twitter account, and you download an app called Acme that can interact with your Twitter account and schedule Tweets in advance. When you try to use the feature in the Acme app, it redirects you to Twitter. Twitter propts you to log on, shows you what permissions the Acme app will access, and then asks if you want to authorize the Acme app to access your Twitter app. If you approve, Twitter sends the Acme app an authorization token. The app may accept and enter the authorization token directly, or you may need to enter it into the app’s settings. When the app accesses the Twitter account, it sends an API message and includes the token. A primary benefit is that you never provide your Twitter credentials to the Acme app. Even if the Acme app is compromised, it does not expose your credentials.
  - **Note:**
    - This doesn’t provide authentication. Instead, it authorizes access to the account.
    - OAuth 2.0 is note backward compatible with OAuth 1.0.
    - OAuth is an authorization framework, not an authentication protocol. It exchanges API messages and uses a token to show that access is authorized.
    - Attacks like XSRF doesn't work on OAuth. However, don't click on any malicious URLs or if a developer fails to verify the `state` parameter or does not use PKCE where required, CSRF attacks could still be possible. One demo of XSRF attack is [here](https://github.com/zedttxj/Web-Security-Exploits/tree/main/Example-1) if you wanna learn more about it.
- **OpenID:** provides decentralized authentication, allowing users to log into multiple unrelated websites with one set of credentials maintained by a third-party service (referred to as an OpenID provider). It’s an open standard but it’s maintained by the OpenID Foundation rather than as an RFC standard.
  - **Explanation:** when users go to an OpenID-enabled website (also known as a relying party), they are prompted to provide their OpenID identity as a URI. The OpenID-enabled website and an OpenID provider exchange data and create a secure channel. The user is then redirected to the OpenID provider and is prompted to provide the password. If correct, the user is redirected back to the OpenID-enabled site (check this [site](http://openidexplained.com/use) to see how this works).
  - **Example:** if your OpenID identifier is bobsmith2021.myopenid.com, that’s what you have to enter. In contrast, other methods exchange data behind the scenes, so it isn’t as obvious what method is being used.
  - OIDC (OpenID Connect): an authentication layer using the OAuth 2.0 authorization framework. It’s maintained by the OpenID Foundation. It builds on the technologies created with OpenID but uses a JWT (or ID token). OIDC uses a web service to retrieve the JWT. In addition to providing authentication, the JWT can also include profile information about the user.
    - Practice on understanding JWT: this [practice](https://play.picoctf.org/practice/challenge/236?category=1&page=4) can help you gain more understanding about JWT (also where I first learned about it).
    - Example: Most of this occurs behind the scenes, but you can see it in action by logging onto eBay with a Google account. These processes and interfaces change over time, but the general steps are as follows:
      1. If you don’t have a Google account, create one first.
      2. Ensure you’re logged out of eBay and Google, go to ebay.com, and click Sign In.
      3. Click Continue With Google. A dialog box opens, prompting you to enter your Google email. It also indicates what Google will share with ebay.com.
      4. Enter your email address and press Enter.
      5. Enter your password and click Next.
      6. If you’ve enabled 2-Step Verification on your Google account, you’ll be prompted to get the code and enter it.
      7. You don’t need to complete the creation of an eBay account with your Google account. However, if you choose to do so, click the Create Account button. You’ll now be logged on to eBay using your Google account. If you log out of eBay and try to log on again, all you need to do is click Sign In and then click Continue with Google. As long as you’re still logged on with Google, you’ll be logged in to eBay without any more steps.

**Comparing SAML, OAuth, OpenID, and OIDC:**
- SAML:
  - SAML 2.0 is an open XML-based standard
  - OASIS adopted it as a standard in 2005
  - It utilizes three entities: a principal (such as a user), a service provider (such as a website), and an identity provider (a third party that holds the authentication and authorization information)
  - It can provide authentication, authorization, and attribute information on the principal
- OAuth:
  - It’s an authorization framework, not an authentication protocol
  - RFC 6749 describes OAuth 2.0
  - It exchanges information using APIs
  - An app obtains an access token from an identity provider
  - Later, the app includes the access token for authorization
- OpenID
  - OpenID is an authentication standard
  - It is maintained by the OpenID Foundation
  - An OpenID provider provides decentralized authentication
  - Users enter their Open ID identifier (such as bobsmith2021.myopenid.com) on a site and the OpenID provider verifies the identifier.
- OIDC
  - OIDC is an authentication layer using OAuth 2.0
  - It builds on the OpenID authentication standard
  - It provides both authentication and authorization
  - It builds on OpenID but uses a JWT.

## Implementing SSO on Internal Networks
