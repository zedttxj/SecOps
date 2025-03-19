# SecOps
### Introduction to Backstory
- Link: https://medium.com/chronicle-blog/introducing-backstory-45dd9b4d4a6d
- To check project ID: `gcloud config list project` (on gcloud console)
- Log-in to ui link: https://cloud.google.com/chronicle/docs/log-in-to-ui  
### Create Web and Mobile Apps - Chronicle Demo
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

### Disable Key Creation Policy in Organization Level
- Recommendations: Check the list of Active policies and disable it. That would be faster
### Signing the CSR with one of the CA
```linux
- Install the certbot: `sudo apt install certbot`
- Use this command to sign the CSR (x.509 convention): `sudo certbot certonly --manual --preferred-challenges dns -d noveltellers.com`
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

#### Make API calls:
- List of available endpoints for REST API calls: https://cloud.google.com/chronicle/docs/reference/rest?_gl=1*tad13c*_ga*MTkxNjU0Nzc2Ny4xNzQxNDY4NTE3*_ga_WH2QY8WWF5*MTc0MjMxMzYzMS4xMS4xLjE3NDIzMTU4NzIuNDMuMC4w  
- Example code:
  ```python3
    import requests
  
    # Set up the API endpoint and request headers with the access token
    endpoint_url = "https://backstory.googleapis.com/v1/your_endpoint_here"  # Replace with actual API endpoint
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
