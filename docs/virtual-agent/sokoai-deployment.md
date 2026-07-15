# Step 5: Deploying the Agent

In this step, you will publish the SokoAI agent and embed it into the SokoAI marketplace website using a web widget.

---

## Open the Publish Panel

1. Navigate to the top of your Agent Builder page and click the highlighted **Publish** icon in the top navigation bar.

    <!-- ADD IMAGE: image57 - Publish icon in top nav -->

2. This takes you to the deployment options page. Select **Create web widget**.

    <!-- ADD IMAGE: image58 - Create web widget selection -->

---

## Configure the Web Widget

A configuration page opens. Complete the following settings:

1. **Widget name** — Enter `soko-deploy`.

2. **Deployment version** — Click the existing deployment version shown, or click **Create a new version** if none exists.

    ![Select deployment version](assets/agent deploy name + create.png)

3. **Input method** — Switch the agent's input method to enable **Text input**.

4. **Agent title** — Enter the title that will appear on the chat widget when it is live on the website:

    ```
    SokoAI Agent
    ```

5. **Public access** — Enable public access to allow unauthenticated users.

6. Click **Create Channel** to complete web widget creation.

    <!-- ADD IMAGE: image60 - Create Channel button and final config -->

---

## Copy the Deployment ID

1. After the channel is created, deployment instructions will be displayed.

2. Using the **copy icon**, copy the **Deployment ID**.

    <!-- ADD IMAGE: image61 - Deployment instructions with copy icon -->

    !!! tip
        You will use this Deployment ID as an environment variable when redeploying the website.

---

## Redeploy the Website

1. Go to your **Repository settings**.
2. Select **Secrets and variables → Actions**.
3. Update the secret `FRONTEND_NEXT_PUBLIC_CHAT_DEPLOYMENT` with the Deployment ID you copied.

    <!-- ADD IMAGE: image62 - GitHub secrets update -->

4. Trigger a redeployment of the website.

5. Once deployed, the **SokoAI Agent** chat widget will appear in the bottom right corner of the website.

---

!!! success "Step 5 Complete"
    The SokoAI agent is now live and embedded in the website. Proceed to testing to verify the full conversation flow.

---

<div class="page-nav">
  <div class="nav-item">
    <a href="../sokoai-agent-instructions/" class="btn-secondary">← Previous: Agent Instructions</a>
  </div>
  <div class="nav-item">
    <span><strong>Section 30</strong> - SokoAI: Deployment</span>
  </div>
  <div class="nav-item">
    <a href="../sokoai-testing/" class="btn-primary">Next: Testing →</a>
  </div>
</div>
