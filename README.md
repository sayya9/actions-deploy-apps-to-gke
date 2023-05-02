# Prerequisites
* Create a Service Account:
```
export PROJECT_ID=your-project-id
gcloud iam service-accounts create github-actions --display-name=github-actions
```

* Grant the Service Account the IAM role `Storage Admin` which has least privilege access for publishing images to GCR:
```
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
        --member=serviceAccount:github-actions@${PROJECT_ID}.iam.gserviceaccount.com \
        --role=roles/storage.admin
```

* Create the service account key:
```
gcloud iam service-accounts keys create /tmp/github-actions.json \
        --iam-account=github-actions@${PROJECT_ID}.iam.gserviceaccount.com
```
The key will be downloaded to /tmp, and we need to create a repo secret containing the JSON key that we generate from the service account.

* Create a repo secret with the name of `GH_ACTIONS_SA_KEY`:

1. In your repo navigation to Settings > Secrets and variables > Actions
1. Under “Actions secrets and variables select” `New repository secret`
