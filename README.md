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
