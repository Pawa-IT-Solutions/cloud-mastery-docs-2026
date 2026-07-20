# Prepare Backend Repo

### Step 1: Fork the Backend Repository

You need your own copy of the application repository to make changes. This is done by "forking" it.

1.  Access the Cloud Mastery backend repository here:
    [https://github.com/Pawa-IT-Solutions/cloud-mastery-backend](https://github.com/Pawa-IT-Solutions/cloud-mastery-backend)

2.  Click the **Fork** button in the top-right corner.

    ![Fork the Backend Repository](assets/images/fork-newrepo.png)

3.  On the "Create a new fork" page, you can leave the details as they are and click **Create fork**.

    ![Create Fork Page](assets/images/click-create-fork.png)

4.  You will be redirected to your own forked copy of the repository. It is now ready!

    ![Forked Repository is Ready](assets/images/cloud-mastery-cloned-repo.png)

---

### Step 2: Clone the Repository to Cloud Shell

Now you can clone your forked repository.

1.  In GitHub, navigate to your forked `cloud-mastery-backend` repository. Click the green **<> Code** button, select the **SSH** tab, and copy the SSH URL.

    ![Copy SSH Clone URL](assets/images/clone-repository.png)

2.  Go back to your **Cloud Shell** terminal and run the `git clone` command, pasting the URL you just copied.
    Replace the URL with your own forked repository SSH URL

    ```
    git clone git@github.com:austinkaruru1/cloud-mastery-backend.git
    ```

3.  When prompted `Are you sure you want to continue connecting (yes/no/[fingerprint])?`, type `yes` and press `Enter`.

    ![Git Clone Output in Cloud Shell](assets/images/cloud-mastery-cloned-repo.png)

4.  Navigate into the newly created directory and list its contents.

    ```
    cd cloud-mastery-backend && ls -l
    ```
    ![List Cloned Repository Files](assets/images/cloud-mastery-cloned-repo.png)

---

## Next Steps

**Great job!** Your GitHub account is configured, and you have successfully cloned the backend application code into your Cloud Shell. Next, we will set up the continuous integration pipeline using Cloud Build.

---
<div class="page-nav">
  <div class="nav-item">
    <a href="../setup-github/" class="btn-secondary">← Previous: Setup Github</a>
  </div>
  <div class="nav-item">
    <span><strong>Prepare Backend Repo </strong></span>
  </div>
  <div class="nav-item">
    <a href="../setup-backend-pipeline" class="btn-primary">Next: Setup Backend Pipeline →</a>
  </div>
</div>

---


