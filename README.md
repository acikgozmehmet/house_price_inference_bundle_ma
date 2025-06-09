# How to Deploy a Streamlit Databricks App for Machine Learning Inferences using  Databricks Asset Bundles


This guide will walk you through deploying a machine learning inference app with a 
Streamlit user interface on Databricks using Databricks Asset Bundles (DABs). 
You’ll learn how to set up your project, configure your bundle, deploy, and run your app.

---

## Prerequisites

- **Databricks Workspace**: You have access to a Databricks workspace.
- **Databricks CLI**: Installed and configured, version `0.250.0` or higher.
- **Git**: Installed on your local machine.

---

## 1. Initialize Your Bundle Project

Start by creating a new project using the official Streamlit template:

```bash
databricks bundle init https://github.com/databricks/bundle-examples --template-dir contrib/templates/streamlit-app
```

This will scaffold a directory with example code and configuration files. 
Feel free to update your code and configuration accordingly.!


---

## 2. Develop and Test Locally

- Place your Streamlit app code in the folder specified by `source_code_path` in the bundle (e.g., `./app`).

- Update ./app/app.yml with the following

```text
command:
  - "streamlit"
  - "run"
  - "app.py"
```

- Update ./app/requirements.txt with your dependencies

---

## 3. Configure Your Bundle

Edit the `databricks.yml` file at the root of your project. Here’s a minimal example:

```
bundle:
  name: house_price_inference_bundle

resources:
  apps:
    inference_app:
      name: "inference-app"
      source_code_path: ../app
      description: A Streamlit app for ML inference on Databricks

targets:
  dev:
    mode: development
    default: true
    workspace:
      host: https://
    permissions:
      - user_name: 
        level: CAN_MANAGE

  prod:
    mode: production
    workspace:
      host: https://
    root_path: /Workspace/Users//.bundle/${bundle.name}/${bundle.target}
    permissions:
      - user_name: 
        level: CAN_MANAGE
```

**Tips:**
- Double-check all app names for typos. The name under `resources.apps` (e.g., `ml-streamlit-app`) must match the name you use when running or deploying the app.
- Replace `` and `` with your actual workspace URL and email.

---

## 4. Validate and Deploy the Bundle

Validate your configuration:

```
databricks bundle validate
```

Deploy your app to Databricks (default is `dev`):

```
databricks bundle deploy
```

To deploy to production:

```
databricks bundle deploy -t prod
```

---

## 5. Run and Access Your App

Start your app in the workspace:

```
databricks bundle run inference_app
```

Get the app’s URL:

```
databricks bundle summary
```

Open the provided URL in your browser to use your deployed app.

---

## 6. Update and Redeploy

After making changes to your app, repeat the `validate` and `deploy` steps to update your deployment.

---

## Troubleshooting

- **App Not Found Error:**  
  If you get `App with name ml-streamlit-appp does not exist or is deleted`, check for typos in your app name in both `databricks.yml` and your commands.
- **Deployment Errors:**  
  Ensure you have run `databricks bundle deploy` before trying to run the app.
- **CLI Version Issues:**  
  Make sure you are using Databricks CLI version `0.250.0` or higher.

---

## Summary Table

-------------------------------------------------------------------------------------------------------------------------
| Step    | Command/Action                                              | Description                                   |
|---------|-------------------------------------------------------------|-----------------------------------------------|
| 1       | `databricks bundle init ...`                                | Initialize project with Streamlit template    |
| 2       | Edit `databricks.yml`                                       | Configure app, environments, permissions      |
| 3       | `databricks bundle validate`   `databricks bundle deploy`   | Validate and deploy to Databricks             |
| 4       | `databricks bundle run ...`    `databricks bundle summary`  | Launch and access your app                    |
| 5       | Repeat steps 2–5                                            | Update and redeploy as needed                 |
-------------------------------------------------------------------------------------------------------------------------

---

## References

- [Databricks Asset Bundles Documentation](https://docs.databricks.com/en/dev-tools/bundles/index.html)
- [Databricks Apps (Streamlit) Documentation](https://docs.databricks.com/en/apps/index.html)
