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
Network access methods allow users to access internal networks from remote locations (such as at home). Kerberos is the most common. Two common remote access protocols are RADIUS and TACACS+. In addition to supporting SSO, RADIUS and TACS+ provider authentication, authorization, and account.  
- **AAA Protocols (authentication, authorization, and accounting):** provide centralized access control with remote access systems such as VPNs. They help protect internal LAN authentication systems and other servers from remote attacks.
  - Example: If you are using a separate system for remote access, a successful attack on the system only affects the remote access users. Attacker won’t have access to internal accounts. They ensure that users have valid credentials to authenticate and verify that they are authorized to connect to the remote access server based on the user’s proven identity.
  - Additionally, the accounting element can track the user’s network resource usage, which can be used for billing purposes.
- **Kerberos:** an important authentication system to know for the CISSP exam and a well-known ticket system.
  - Ticket authentication: a mechanism that employs a third-party entity to prove identification and provide authentication.
  - Primary purpose of Kerberos: authentication. Keberos uses users’ proven identity to issue tickets, and user accounts present these tickets when access resources.
  - Offering a SSO solution for users and protect logon credentials.
  - Kerberos verions 5 relies on symmetric-key cryptography (secret-key cryptography) using AES. Kerberos provides confidentiality and integrity for authentication traffic using end-to-end security and helps protect against eavesdropping and replay attacks.
  - Many of Kerberos roles are on a single server, but they can be installed on different servers. Larger networks sometimes separate them to increase performance, but small networks typically have one kerberos server performing all of the different roles.
  - List of important elements used in Kerberos:
    1. Key Distribution Center: the trusted third party that provides authentication services. Kerberos uses symmetric-key cryptography to authenticate clients to servers. All clients and servers are registered with the KDC, and it maintains the secret keys for all network members.
    2. Kerberos Authentication Server: the authentication server hosts the functions of the KDC: a ticket-granting service (TGS) and an authentication service (AS). However, it’s possible to host the ticket-granting service on another server. The AS verifies or rejects the authenticity and timeliness of tickets. This server is often called the KDC.
    3. Ticket (sometimes called server ticket, ST): an encrypted message that provides proof that a subject is authorized to access an object. Subjects (like users) request tickets to access objects (such as files), and if they have authenticated and are authorized to access the object, Kerberos issues them a ticket (having specific lifetimes and usage parameters). If the ticket expires, a client must request a renewal or a new ticket.
    4. Ticket-Granting Ticket (TGT): provides proof that a subject has authenticated through a KDC and is authorized to request tickets to access other objects. A TGT is encrypted and includes a symmetric key, an expiration time, and the user’s IP address.
    5. Kerberos Principal: Kerberos issues tickets to Kerberos principals. A Kerberos principal is typically a user but can be any entity that can request a ticket.
    6. Kerberos Realm: a logical area controlled or ruled by Kerberos. Principals within the realm can request tickets from Kerberos, and Kerberos can issue tickets to principals in the realm.
  - The database of accounts could be stored in a directory service such as Microsoft’s Active Directory (AD), which enables Kerberos by default.
  - The Kerberos login process works as follows:
    1. The user types a username and password into the client
    2. The client generates a request with these credentials to the KDC. This request transmission is encrypted
    3. The KDC verifies the username against its database of known credentials
    4. The KDC generates a session key that will be used by the client and the Kerberos server. It encrypts this with a hash of the user’s password. The KDC also generates an encrypted timestamped TGT.
    5. The KDC then transmits the encrypted session key and the encrypted timestamped TGT to the client.
    6. The client installs the TGT for use until it expires. The client also decrypts the session key using a hash of the user’s password. 
  - When a client wants to access an object, it must request a ticket through the Kerberos server:
    1. The client sends its TGT back to the KDC with a request for access to the resource.
    2. The KDC verifies that the TGT is valid and checks its access control matrix to verify that the user has sufficient privileges to access the requested resource.
    3. The KDC generates a service ticket and sends it to the client.
    4. The client sends the ticket to the server or service hosting the resource.
    5. The server or service hosting the resource verifies the validity of the ticket with the KDC.
    6. Once identity and authorization are verified, Kerberos activity is complete. The server or service host then opens a session with the client and begins communications or data transmission.
  - Kerberos is a versatile authentication mechanism that works over local LANs, remote access, and client/server resource requests. However, it presents a single point of failure (the KDC). If the KDC is compromised, the secret for every system on the network is also compromised. If a KDC goes offline, no subject authentication can occur.
  - It also has strict time requirements, and the default configuration requires that all systems be time-synchronized within 5 minutes of each other. Administrators often configure a time synchronization system within a network. In an Active Directory domain, one domain controller (DC) synchronizes its time with an external Network Time Protocol (NTP) server. All other DCs synchronize their time with the first DC. All other systems synchronize their time with one of the DCs when they log on.
- **RADIUS (Remote Authentication Dial-in User Service):** centralizes authentication for remote access connections (like with VPNs or dial-up access).
  - It’s typically used when an organization has more than one network access server (or remote access server). A user can connect to any network access server, which then passes on the user’s credentials to the RADIUS server to verify authentication and authorization and to track accounting. In this context, the network access server is the RADIUS client, and a RADIUS server acts as an authentication server.
  - Many internet servier providers (ISPs) use RADIUS for authentication. User can access the ISP from anywhere, and the ISP server then forwards the user’s connection request to the RADIUS server.
  - Organizations can also use RAIDUS, and organizations often implement it with location-based security. Althought it itsn’t as comon today, some users still have Integrated Services Digital Network (ISDN) dial-up lines and use them to connect to VPNs. Users call in, and after authentication, the RADIUS server terminates the connection and initiates a call back to the user’s predefined phone number.
  - RADIUS uses the UDP by default and encrypts only the password’s exchange. RADIUS can use other protocols to encrypt the data session. The current version is defined in RFC 2865. RFC 6614, designated as Experimental, defines how RADIUS can use TLS over TCP. When using TLS, RADIUS uses TCP port 2083. RADIUS uses UDP port 1812 for RADIUS messages and UDP port 1813 for RADIUS Accounting message. Interestingly, an early draft of RADIUS/TLS was called TLS encryption for RADIUS over TCP (RadSec). However, RFC 6614 omitted the parenthetical RadSec. Radiator Software still sells Radiator and refers to RadSec as “secure, reliable RADIUS proxying.” However, it’s alsos possible to use RADIUS/TLS to encrypt the entire session.
- **TACACS+ (Terminal Access Controller Access Control System Plus):** provides several improvements over the earlier versions and over RADIUS. It separates authentication, authorization, and accounting into separate processes, which can be hosted on three different servers if desired. Additionally, it encrypts all of the authentication information, not just the password, as RADIUS does. TACACS+ uses TCP port 49, providing a higher level of reliability for the packet transmissions.
