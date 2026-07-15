# Setup CI/CD Pipeline — Workload Identity Federation & GitHub Secrets

In this section you will establish a secure, keyless connection between your GCP project and GitHub using Workload Identity Federation, then configure the GitHub Actions secrets that power the automated deployment pipeline.

---

## What is Workload Identity Federation?

Workload Identity Federation is a secure method for granting external identities (like GitHub Actions) temporary access to Google Cloud resources **without using long-lived service account keys**. It works by having Google Cloud trust an external identity provider (GitHub) and map the external identity's token to a specific GCP Service Account. This eliminates the security risk of storing static keys, ensuring a keyless CI/CD pipeline for deploying to Cloud Run.

---

## Before You Start — Gather Your Variables

You need three values from your GCP project. Find them on the GCP Welcome page:
[https://console.cloud.google.com/welcome](https://console.cloud.google.com/welcome)

| Variable | Where to find it |
|---|---|
| `PROJECT_ID` | GCP Welcome page — Project ID field |
| `PROJECT_NUMBER` | GCP Welcome page — Project number field |
| `GITHUB_REPO` | `[YOUR_USERNAME]/cloud-mastery-ecommerce-2026` |

---

## Step 1: Open Cloud Shell

1. In the GCP Console, click the **Activate Cloud Shell** icon in the top-right toolbar.

    ![Activate Cloud Shell](assets/images/activate-cloudshell.png)

2. Click **Authorize** on the pop-up screen if prompted.

    ![Cloud Shell ready](assets/images/opened-cloudshell.png)

---

## Step 2: Set Environment Variables

=== "Cloud Shell / Linux / macOS"

    ```shell
    export PROJECT_ID="[YOUR_PROJECT_ID]"
    export GITHUB_REPO="[YOUR_USERNAME]/cloud-mastery-ecommerce-2026"
    export PROJECT_NUMBER="[YOUR_PROJECT_NUMBER]"
    ```

=== "Windows (Command Prompt)"

    ```shell
    set PROJECT_ID="[YOUR_PROJECT_ID]"
    set GITHUB_REPO="[YOUR_USERNAME]/cloud-mastery-ecommerce-2026"
    set PROJECT_NUMBER="[YOUR_PROJECT_NUMBER]"
    ```

---

## Step 3: Create the Workload Identity Pool

A Pool is a container for your external identity providers.

=== "Cloud Shell / Linux / macOS"

    ```shell
    gcloud iam workload-identity-pools create "github-pool-v1" \
        --project="$PROJECT_ID" \
        --location="global" \
        --display-name="GitHub Pool V1"
    ```

=== "Windows (Command Prompt)"

    ```shell
    gcloud iam workload-identity-pools create "github-pool-v1" --project="%PROJECT_ID%" --location="global" --display-name="GitHub Pool V1"
    ```

---

## Step 4: Create the OIDC Provider

This command tells Google Cloud to trust GitHub's identity tokens.

!!! warning "Action Required"
    Replace `'Pawa-IT-Solutions'` in the `--attribute-condition` flag below with **your own GitHub username**.

=== "Cloud Shell / Linux / macOS"

    ```shell
    gcloud iam workload-identity-pools providers create-oidc "github-provider-v1" \
        --project="$PROJECT_ID" \
        --location="global" \
        --workload-identity-pool="github-pool-v1" \
        --display-name="GitHub Provider V1" \
        --issuer-uri="https://token.actions.githubusercontent.com" \
        --attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository,attribute.actor=assertion.actor" \
        --attribute-condition="assertion.repository_owner == '[YOUR_GITHUB_USERNAME]'"
    ```

=== "Windows (Command Prompt)"

    ```shell
    gcloud iam workload-identity-pools providers create-oidc "github-provider-v1" --project="%PROJECT_ID%" --location="global" --workload-identity-pool="github-pool-v1" --display-name="GitHub Provider V1" --issuer-uri="https://token.actions.githubusercontent.com" --attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository,attribute.actor=assertion.actor" --attribute-condition="assertion.repository_owner == '[YOUR_GITHUB_USERNAME]'"
    ```

---

## Step 5: Bind GitHub to the Workload Identity Pool

This allows GitHub to establish a connection with your GCP project.

=== "Cloud Shell / Linux / macOS"

    ```shell
    gcloud iam service-accounts add-iam-policy-binding \
        "github-deploy-sa@$PROJECT_ID.iam.gserviceaccount.com" \
        --project="$PROJECT_ID" \
        --role="roles/iam.serviceAccountTokenCreator" \
        --member="principalSet://iam.googleapis.com/projects/$PROJECT_NUMBER/locations/global/workloadIdentityPools/github-pool-v1/attribute.repository/$GITHUB_REPO"
    ```

=== "Windows (Command Prompt)"

    ```shell
    gcloud iam service-accounts add-iam-policy-binding "github-deploy-sa@%PROJECT_ID%.iam.gserviceaccount.com" --project="%PROJECT_ID%" --role="roles/iam.serviceAccountTokenCreator" --member="principalSet://iam.googleapis.com/projects/%PROJECT_NUMBER%/locations/global/workloadIdentityPools/github-pool-v1/attribute.repository/%GITHUB_REPO%"
    ```

---

## Step 6: Run the Setup Script

The repository includes a helper script (`setup-github-wif.sh` / `setup-github-wif.bat`) that enables the required GCP APIs and configures IAM permissions for the `github-deploy-sa` service account.

### Running from your local machine (Linux/macOS)

1. Open `setup-github-wif.sh` in your IDE and edit the `GITHUB_OWNER` variable to match your GitHub username.

    ![Edit GITHUB_OWNER in the script — Linux/macOS](assets/images/change-githubowner.png)

2. Log in with the email you were given, then make the script executable and run it:

    ```shell
    gcloud auth application-default login
    chmod +x setup-github-wif.sh
    ./setup-github-wif.sh
    ```

### Running from Windows

1. Open `setup-github-wif.bat` in your IDE and update the `GITHUB_OWNER` variable.

    ![Edit GITHUB_OWNER in the script — Windows](assets/images/change-githubowner-windows.png)

2. Run:

    ```shell
    setup-github-wif.bat
    ```

### Running from Cloud Shell

1. Upload `setup-github-wif.sh` to Cloud Shell using the **Upload file** button.

    ![Upload script to Cloud Shell](assets/images/cloudshell-upload.png)

2. Make it executable and run it:

    ```shell
    chmod +x setup-github-wif.sh
    ./setup-github-wif.sh
    ```

### Expected Output

After a successful run you will see:

```
GCP_WORKLOAD_IDENTITY_PROVIDER: projects/[PROJECT_NUMBER]/locations/global/workloadIdentityPools/github-pool-v1/providers/github-provider-v1
GCP_DEPLOYER_SERVICE_ACCOUNT_EMAIL: github-deploy-sa@[PROJECT_ID].iam.gserviceaccount.com
```

**Copy both values** — you will use them in the next step.

---

## Step 7: Confirm the Service Account

In the GCP Console go to **IAM & Admin → Service Accounts** and confirm `github-deploy-sa` was created. Copy its email address.

![github-deploy-sa service account](assets/images/service_acccount-mh.png)

---

## Step 8: Configure GitHub Actions Secrets

GitHub Actions secrets let the pipeline access sensitive configuration without exposing values in source code.

1. In your forked repository on GitHub, click the **Settings** tab.
2. In the left navigation click **Secrets and variables → Actions**.

    ![Open GitHub Actions Secrets](assets/images/open-github-secrets.png)

3. Click **New repository secret** and add each secret below:

    | Secret Name | Value |
    |---|---|
    | `GCP_PROJECT_ID` | Your GCP project ID |
    | `GCP_REGION` | `us-central1` |
    | `GCP_WORKLOAD_IDENTITY_PROVIDER` | `projects/[PROJECT_NUMBER]/locations/global/workloadIdentityPools/github-pool-v1/providers/github-provider-v1` |
    | `GCP_DEPLOYER_SERVICE_ACCOUNT_EMAIL` | `github-deploy-sa@[PROJECT_ID].iam.gserviceaccount.com` |
    | `CLOUD_RUN_BACKEND_SERVICE_NAME` | `soko-backend` |
    | `CLOUD_RUN_FRONTEND_SERVICE_NAME` | `soko-frontend` |
    | `CLOUDSQL_INSTANCE_CONNECTION_NAME` | Copied from the Cloud SQL step |
    | `DB_NAME` | `cloud_mastery_sample` |
    | `DB_USER` | `Cloud_Mastery1` |
    | `DB_PASSWORD` | `Cloud_Mastery2` |
    | `MYSQL_PRISMA_URL` | `mysql://Cloud_Mastery1:Cloud_Mastery2@localhost:3306/cloud_mastery_sample?socket=/cloudsql/[CLOUDSQL_INSTANCE_CONNECTION_NAME]` |
    | `FRONTEND_NEXT_PUBLIC_CART_API_URL` | `https://checkout-service-[PROJECT_NUMBER].us-central1.run.app` |
    | `FRONTEND_NEXT_PUBLIC_CHAT_DEPLOYMENT` | *(provided by trainer)* |
    | `FRONTEND_NEXT_PUBLIC_CHAT_TITLE` | `SokoAI Agent` |
    | `PESAPAL_CONSUMER_KEY` | *(provided by trainer)* |
    | `PESAPAL_CONSUMER_SECRET` | *(provided by trainer)* |
    | `PESAPAL_NOTIFICATION_ID` | *(provided by trainer)* |

4. Once all secrets are added your Secrets page should look like this:

    ![GitHub Actions secrets configured](assets/images/Gihub_Secrets.png)

---

## Step 9: Verify Secret Manager

The setup script also creates a secret in GCP Secret Manager to store the database URL for Cloud Run.

1. In the GCP Console search for **Secret Manager** and confirm a secret named `MYSQL_PRISMA_URL` was created.

    The stored value format is:
    ```
    mysql://Cloud_Mastery1:Cloud_Mastery2@localhost:3306/cloud_mastery_sample?socket=/cloudsql/[CLOUDSQL_INSTANCE_CONNECTION_NAME]
    ```

---

## What's Next

You have set up a secure, keyless connection between GCP and GitHub and configured all deployment secrets. In the next section you will trigger the pipeline by pushing a commit.

---

<div class="page-nav">
  <div class="nav-item">
    <a href="../setup-github/" class="btn-secondary">← Previous: Setup GitHub</a>
  </div>
  <div class="nav-item">
    <span><strong>CI/CD Pipeline Setup</strong></span>
  </div>
  <div class="nav-item">
    <a href="../setup-frontend-pipeline" class="btn-primary">Next: Trigger the Pipeline →</a>
  </div>
</div>
