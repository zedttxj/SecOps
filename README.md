# SecOps
### Disable Key Creation Policy in Organization Level
```
anhminh_nguyen@cloudshell:~ (liquid-fort-453121-g5)$ history
    1  gcloud auth login
    2  gcloud iam service-accounts keys create key-file.json   --iam-account chronicle-api-service-account@liquid-fort-453121-g5.iam.gserviceaccount.com
    3  gcloud org-policies list --filter="constraint=constraints/iam.managed.disableServiceAccountKeyCreation"
    4  gcloud org-policies describe constraints/iam.managed.disableServiceAccountKeyCreation --effective
    5  gcloud org-policies list --filter="constraint=constraints/iam.managed.disableServiceAccountKeyCreation" --project=liquid-fort-453121-g5
    6  gcloud organizations list
    7  950157668093
    8  gcloud org-policies describe constraints/iam.managed.disableServiceAccountKeyCreation --organization=ORG_ID --effective
    9  gcloud org-policies describe constraints/iam.managed.disableServiceAccountKeyCreation --organization=950157668093 --effective
   10  gcloud organizations get-iam-policy 950157668093
   11  gcloud org-policies reset constraints/iam.managed.disableServiceAccountKeyCreation --organization=950157668093
   12  gcloud org-policies describe constraints/iam.managed.disableServiceAccountKeyCreation --organization=ORG_ID --effective
   13  gcloud org-policies describe constraints/iam.managed.disableServiceAccountKeyCreation --organization=950157668093 --effective
   14  history
anhminh_nguyen@cloudshell:~ (liquid-fort-453121-g5)$ 
```
- Recommendations: Check the list of Active policies and disable it. That would be faster

### Signing the CSR with one of the CA

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
