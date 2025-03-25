# Google SecOps
### Introduction to Backstory
- Link: https://medium.com/chronicle-blog/introducing-backstory-45dd9b4d4a6d
- To check project ID: `gcloud config list project` (on gcloud console)
- Log-in to ui link: https://cloud.google.com/chronicle/docs/log-in-to-ui
### What is IdP (Identity Provider)?
In the case of device fingerprinting: Users can register their devices with the organization and associate them with their user accounts. The device authentication system then captures the characteristics of the device (often accomplished by having the user access a web page with the device). The registration system then identifies the device using attributes such as the operating system and version, web browser, browser fonts, browser plug-ins (extensions), time zone, data storage, screen resolution, cookie settings, and HTTP headers. Even though some of these characteristics change over time, this has proven to be a successful device authentication method. Organizations often use third-party tools like SecureAuth Identity Provider (**IdP**). MDM systems use contexts-aware authentication methods to identify devices. 802.1X is another method used for device authentication (implemented by MDM or NAC solutions).
### What is Service Account?
Many services require authentication. A service account is simply a user account that an administrator created for a service or application instead of a person. It’s common to create a service account for third-party tools monitoring email in Microsoft’s Exchange Server or Chronicle API. These third-party tools typically need permission to scan all mailboxes looking for spam, malware, potential data exfiltration attempts, and more.
### Create IdP App - Chronicle Demo
![{14F1AD88-9810-450E-AA47-2AE5FC8447CF}](https://github.com/user-attachments/assets/2f4b9615-51a2-4d33-85f4-310f5de20505)
### Check out for subdomains in Chronicle
#### 1. Check for Chronicle API
```linux
  gcloud services list --enabled | grep chronicle
  Reauthentication required.
  Please enter your password:
  Reauthentication successful.
  chronicle.googleapis.com                Chronicle API
```
If `chronicle.googleapis.com` appears, it means Chronicle is enabled in your project.
#### 2. Check for Google SecOps page
In `console.cloud.google.com`, click on the menu (3 horizontal slashes) and then go to `Security` > `Google SecOps`. Recommend to access in Incognito mode to rule out any extension issues. Looks something like this:  
![{DBDFE784-2A03-40A1-A072-ED7698EEA8B2}](https://github.com/user-attachments/assets/6e4b97b9-6617-4524-ac61-7c292f7f4fb5)  
If this page shows up, it means you haven't set up any instance yet. You would have to contact the [Google Cloud Security](https://cloud.google.com/contact/security?hl=en) to create a subdomain for Google SecOps (Chronicle Backstory). Chronicle Backstory isn’t publicly available for individual study (not in the API library). It’s locked behind enterprise sales, which makes it difficult for students or independent learners to get hands-on experience.

### Disable Key Creation Policy in Organization Level
- Recommendations: Check the list of Active policies and disable it. That would be faster. This helps you create the key. After creating the key for the Chronicle API project, the key will be automatically downloaded into your computer. Use that key for `Interacting with Chronicle API using service account key via REST API`.
### Signing the CSR with one of the CA
- Install the certbot: `sudo apt install certbot`
- Use this command to sign the CSR (x.509 convention): `sudo certbot certonly --manual --preferred-challenges dns -d <my domain name>`
```linux
[sudo] password for zedttxj:
/usr/local/lib/python3.10/dist-packages/requests-2.20.0-py3.10.egg/requests/__init__.py:89: RequestsDependencyWarning: urllib3 (1.26.5) or chardet (4.0.0) doesn't match a supported version!
  warnings.warn("urllib3 ({}) or chardet ({}) doesn't match a supported "
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): c
An e-mail address or --register-unsafely-without-email must be provided.
Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.
zedttxj@LAPTOP-AIAK8VAQ:~/test/t$
```
### Interacting with Chronicle API using service account key via REST API  

#### Setting up authentication
- Example code:  
    ```python3
    from google.oauth2 import service_account
    from google.auth.transport.requests import Request
    import google.auth
    import json
    
    # Path to your service account key file
    key_path = 'path/to/your-service-account-key.json'
    
    # Scopes required for Chronicle API
    SCOPES = ["https://www.googleapis.com/auth/cloud-platform"]
    
    # Load the service account credentials
    credentials = service_account.Credentials.from_service_account_file(
        key_path,
        scopes=SCOPES
    )
    
    # Ensure the token is refreshed
    credentials.refresh(Request())
    
    print(f"Authenticated with access token: {credentials.token}")
    ```
- The output is like this:  
  ![{B5059D1A-7076-4D54-8299-D1182455A186}](https://github.com/user-attachments/assets/c01e1dc5-78b5-48d3-bb18-778beb86c459)
- Note: It's recommended to have `https://www.googleapis.com/auth/chronicle-backstory` instead of using `cloud-platform` (for general purpose).

#### Enable Backstory API for your project (in my case is Chronicle API project)
- Notice: Google SecOps (Chronicle Backstory) isn’t publicly available for individual study. It’s locked behind enterprise sales, which makes it difficult for students or independent learners to get hands-on experience. You gotta contact the [Google Cloud Security](https://cloud.google.com/contact/security?hl=en).
- Before enabling, I tried `SetUIState` with this example code:
  ```python3
  import requests
  
  # The API endpoint for querying events
  endpoint_url = "https://backstory.googleapis.com/v1/partner/customer/setuistate:state"
  
  # Query body (adjust filter and other parameters as needed)
  data = {
      "customer_code": "CODE",  # Replace with actual customer code
      "state": True  # True to enable, False to disable
  }
  
  # Headers for the request, including the access token
  headers = {
      "Authorization": f"Bearer {credentials.token}",
      "Content-Type": "application/json"
  }
  
  # Make the POST request
  response = requests.post(endpoint_url, headers=headers)
  
  # Check if the request was successful
  if response.status_code == 200:
      print("UI state successfully updated!")
      print(json.dumps(response.json(), indent=2))
  else:
      print(f"Error: {response.status_code}, {response.text}")
  ```
- The data is retrieved with 403 exit code:
  ```json
  Error: 403, {
    "error": {
      "code": 403,
      "message": "Backstory API has not been used in project <my project id> before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=<my project id> then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry.",
      "status": "PERMISSION_DENIED",
      "details": [
        {
          "@type": "type.googleapis.com/google.rpc.ErrorInfo",
          "reason": "SERVICE_DISABLED",
          "domain": "googleapis.com",
          "metadata": {
            "service": "backstory.googleapis.com",
            "activationUrl": "https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=<my project id>",
            "consumer": "projects/<my project id>",
            "containerInfo": "<my project id>",
            "serviceTitle": "Backstory API"
          }
        },
        {
          "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
          "locale": "en-US",
          "message": "Backstory API has not been used in project <my project id> before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=<my project id> then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry."
        },
        {
          "@type": "type.googleapis.com/google.rpc.Help",
          "links": [
            {
              "description": "Google developers console API activation",
              "url": "https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=<my project id>"
            }
          ]
        }
      ]
    }
  }
  ```
  - Note: you can get a list of your projects' IDs with the gcloud CLI like this: `gcloud projects list`
  - Apparently, I couldn't enable my Backstory API when I access the [link](https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=<my project id>). I also tried `gcloud services enable backstory.googleapis.com --project=<my project id>`:
  ```gcloud
  ERROR: (gcloud.services.enable) PERMISSION_DENIED: Permission denied to enable service [backstory.googleapis.com]
  Help Token: <the token>. This command is authenticated as <owner's email> which is the active account specified by the [core/account] property
  - '@type': type.googleapis.com/google.rpc.PreconditionFailure
    violations:
    - subject: ...
      type: googleapis.com
  - '@type': type.googleapis.com/google.rpc.ErrorInfo
    domain: serviceusage.googleapis.com
    metadata:
      permission: servicemanagement.services.bind
      resource: '<my project id>'
      service: servicemanagement.googleapis.com
    reason: AUTH_PERMISSION_DENIED
  ```
  - Solution: you gotta enable it with the OAuth token from your `.json` key. Simply put, change the `endpoin_url` into "https://console.developers.google.com/apis/api/backstory.googleapis.com/overview?project=705233697694" so that it includes the access token:
  ```python3
  # Headers for the request, including the access token
  headers = {
      "Authorization": f"Bearer {credentials.token}",
      "Content-Type": "application/json"
  }
  
  # Make the POST request
  response = requests.get(endpoint_url, headers=headers)
  ```
  And we enabled Backstory API for our project:
  ```linux
  zedttxj@LAPTOP-AIAK8VAQ:~/test$ python3 restapi.py
  UI state successfully updated!
  ```
#### Make API calls:
- List of available endpoints for REST API calls: https://cloud.google.com/chronicle/docs/reference/rest?_gl=1*tad13c*_ga*MTkxNjU0Nzc2Ny4xNzQxNDY4NTE3*_ga_WH2QY8WWF5*MTc0MjMxMzYzMS4xMS4xLjE3NDIzMTU4NzIuNDMuMC4w
- API sample with python: https://github.com/chronicle/api-samples-python  
- Example code:
  ```python3
    import requests
  
    # Set up the API endpoint and request headers with the access token
    endpoint_url = "https://backstory.googleapis.com/v1/partner/customer/forwarder/getconfigs?"  # Replace with actual API endpoint
    headers = {
        "Authorization": f"Bearer {credentials.token}",
        "Content-Type": "application/json",
    }
    
    # Make a GET or POST request to Chronicle API
    response = requests.get(endpoint_url, headers=headers)
    
    if response.status_code == 200:
        print("Response data:", json.dumps(response.json(), indent=2))
    else:
        print(f"Error: {response.status_code}, {response.text}")
  ```
- Note: change your url according to your region. In my case, I use `backstory.googleapis.com` for United States Multi-Region. You can check the list of available region here: https://cloud.google.com/chronicle/docs/reference/customer-management-api
