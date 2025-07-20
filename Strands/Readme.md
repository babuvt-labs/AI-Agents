Great! Here’s a **step-by-step guide** to run a Strands Agent on Azure, using **Azure OpenAI** as your LLM provider. This example assumes you have access to [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/ai-services/openai/) and want to run your agent as a simple Python app (e.g., in Azure App Service, a VM, or Container Instance). If you want another setup (like serverless/Azure Functions or AKS), let me know!

---

## **Step 1: Set Up Azure OpenAI**

1. **Get Access:**

   * Request access to Azure OpenAI [here](https://aka.ms/oai/access).
2. **Deploy a Model:**

   * In the Azure Portal, go to your Azure OpenAI resource.
   * Click "Deployments" > "Create new deployment".
   * Choose a model (e.g., `gpt-35-turbo`, `gpt-4`), give it a name (e.g., `my-gpt4`).

---

## **Step 2: Gather Credentials**

* **Endpoint:**
  Example: `https://<your-resource-name>.openai.azure.com/`
* **Deployment Name:**
  Example: `my-gpt4`
* **API Key:**
  From "Keys and Endpoint" in the Azure OpenAI resource.

---

## **Step 3: Prepare Your Local Project**

1. **Create a Python virtual environment (optional but recommended):**

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install Strands Agents and dependencies:**

   ```bash
   pip install strands-agents strands-agents-tools strands-agents-builder openai
   ```

---

## **Step 4: Write Your Agent Code**

Here’s a minimal example using Azure OpenAI:

```python
from strands import Agent
from strands_tools import calculator

# Agent configuration for Azure OpenAI
azure_openai_config = {
    "model_provider": "openai",
    "api_base": "https://<your-resource-name>.openai.azure.com/",
    "api_key": "<your-azure-openai-key>",
    "api_version": "2023-05-15",  # or your deployed API version
    "model": "<your-deployment-name>",  # e.g., "my-gpt4"
}

# Create the agent
agent = Agent(
    tools=[calculator],
    **azure_openai_config
)

# Use the agent
response = agent("What is 123 * 456?")
print(response)
```

> **Replace** `<your-resource-name>`, `<your-azure-openai-key>`, and `<your-deployment-name>` with your actual values.

---

## **Step 5: Set Environment Variables (Recommended)**

Instead of hardcoding secrets, set them as environment variables:

```bash
export OPENAI_API_BASE="https://<your-resource-name>.openai.azure.com/"
export OPENAI_API_KEY="<your-azure-openai-key>"
export OPENAI_API_VERSION="2023-05-15"
export OPENAI_DEPLOYMENT_NAME="<your-deployment-name>"
```

Update your code to read from `os.environ` for security.

---

## **Step 6: Test Locally**

Run your script:

```bash
python agent.py
```

You should see the agent's response in your terminal.

---

## **Step 7: Deploy to Azure (Optional)**

**A. Azure App Service / Container Instance:**

* Push your code to a public repo (GitHub, etc.).
* Set up an App Service or Container, and configure the environment variables in Azure Portal.
* Deploy your code.

**B. Azure Functions (Serverless):**

* Use [Python Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-python).
* Use the above code in your function.
* Bind env vars as application settings.

---

## **Summary Table**

| Step              | What You Do                                      |
| ----------------- | ------------------------------------------------ |
| Azure Portal      | Deploy model, get endpoint and API key           |
| Local Setup       | Install SDK, write Python agent code             |
| Configure         | Plug Azure OpenAI creds into config/env vars     |
| Test              | Run script locally, check agent response         |
| Deploy (optional) | Push to Azure as web app, container, or function |

---
