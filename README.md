# Agentforce Default Automated Deployment for Salesforce

## Project Purpose

This project implements **Agentforce Default** a.k.a. **Einstein Copilot**, your AI-powered assistant for Salesforce. The Copilot is designed to help users with in-org business tasks such as querying records, summarizing data, answering questions, and drafting emails. It leverages Salesforce's GenAI capabilities to provide intelligent, context-aware assistance directly within your Salesforce environment.

## Main Components

- **Bot Definition**  
  The bot is defined in `force-app/main/default/bots/Copilot_for_Salesforce/Copilot_for_Salesforce.bot-meta.xml`.  
  - **Type:** InternalCopilot (for employee use)
  - **Label:** Einstein Copilot
  - **Description:** AI assistant for business tasks like querying records
  - **Context Variables:**  
    - `Object_Api_Name` (Text): The API name of the Salesforce object in context  
    - `Record_Id` (Id): The record ID in context  
  - **Rich Content:** Enabled for enhanced user interactions

- **Planner and Plugins**  
  - **Planner Bundle:** `EmployeeCopilotPlanner` (links the bot to AI planning logic)
  - **Plugin:** `MigrationDefaultTopic` (Agent Topic that defines available functions and instructions)
  - **Functions:** Identify records, answer questions, summarize, query, draft emails, and more

- **Metadata Packaging**  
  All metadata is organized for Salesforce DX, making it easy to deploy and manage across orgs.

## Connecting and Deploying to Salesforce Orgs

You can connect this project to different Salesforce orgs and deploy metadata using the Salesforce DX CLI (`sf`). Below is a typical workflow:

### 1. Authorize Your Orgs

Authorize your production and sandbox orgs using web login:

```sh
sf org:login:web --alias afs007-prod --instance-url https://login.salesforce.com --set-default
sf org:login:web --alias afs007-sdbx --instance-url https://test.salesforce.com --set-default
```

- `--alias` gives your org a friendly name for future commands.
- `--instance-url` specifies the Salesforce environment (`login.salesforce.com` for production, `test.salesforce.com` for sandbox).
- `--set-default` makes this org the default for CLI operations.

### 2. Preview Your Deployment

Before deploying, preview what will be deployed to your org. This helps you see the metadata components that will be deployed or deleted, any potential conflicts, and files that will be ignored.

**Requirements:**  

> - To preview deployments, your org must have source tracking enabled. Source tracking is enabled by default on scratch orgs and on Developer/Developer Pro sandboxes if you enable it in your org setup.  
> - To create and manage scratch orgs, you need Dev Hub enabled in your Salesforce environment.

**How to enable Dev Hub:**  
Go to Salesforce Setup → "Dev Hub" → Enable Dev Hub.

**How to enable Source Tracking in Sandboxes:**  
Go to Salesforce Setup → "Dev Hub" → Enable "Source Tracking in Developer and Developer Pro Sandboxes".  
After enabling, any new or refreshed Developer/Developer Pro sandboxes will have source tracking enabled automatically.

Preview the deployment of source files in a directory, such as `force-app`, to your default org:

```sh
sf project deploy preview --source-dir force-app
```

Other preview examples:

- Preview deployment of all components listed in a manifest:

  ```sh
  sf project deploy preview --manifest path/to/package.xml
  ```

> **Note:**  
> The preview command outputs a table describing what will happen if you run the deployment. It lists components to be deployed or deleted, conflicts, and ignored files. Source tracking is enabled by default on scratch and sandbox orgs, but not on production orgs.

### 3. Deploy Metadata to an Org

Deploy the project metadata (e.g., to production):

```sh
sf project deploy start --source-dir force-app --target-org afs007-prod
```

- `--source-dir` specifies the folder containing your metadata.
- `--target-org` uses the alias you set earlier.

### 4. Verify Deployment

After deployment, log in to your Salesforce org and verify that the Einstein Copilot bot and related metadata are available and configured as expected.

---

For more details on Salesforce DX, see the [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm).
