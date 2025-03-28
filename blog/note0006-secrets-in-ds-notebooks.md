---
slug: secrets-in-ds-notebooks
title: 'Note 6: Manage secrets in different notebooks environments'
authors: ivanignatiev
tags: []
date: 2025-03-29T09:30
---

Look to tutorials for Data Engineers and Data Scientists, responses in Stackoverflow. Hardcoded credentials ([CWE-798](https://cwe.mitre.org/data/definitions/798.html)) are everywhere, if not in code, printed in output cell. Risk to push notebook with api-keys to git or a pipeline is very high.

Static analysis tools `pynblint` and `bandit` does not detect hardcoded secrets in notebooks. Issues can be caught during code review. However, file will be distributed across all team members PCs if merged in some shared branch.

Public services like GitHub and some serverless services ([Scanning AWS Lambda functions with Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/scanning-lambda.html)) can identify [CWE-798](https://cwe.mitre.org/data/definitions/798.html) but it does not reduce responsibilities of notebooks and code owners.

## Local Notebooks

Use environment variables, or `dotenv`, or `keyring`, or password inputs.

```python
secret_in_env = os.environ['ENVIRONMENT_VARIABLE_WITH_SECRET_NAME']
```

Do not print your secret variables, or ensure your cell outputs are clean before committing to `git`.

If you store your secrets in `.env` file, add this file to `.gitignore`.

## Google Colab Secrets

```python
from google.colab import userdata
secret_in_env = userdata.get('{googleColabSecretName}')
```

Figure. Google Colab Secrets:

![Google Colab Secrets](/assets/note0006/google-colab-secrets.png)

## Databricks

```python
username = dbutils.secrets.get(scope = "{databricksSecretNamespace}", key = "{databricksSecretName}")
```

[Tutorial: Create and use a Databricks secret](https://learn.microsoft.com/en-us/azure/databricks/security/secrets/example-secret-workflow)

## Azure DevOps

In MLOps, ModelOps in pipelines secrets can be stored over Variables Groups. Additionally, Variable Group can be linked to Azure KeyVault.

From Variables secrets can be populated as environment variables in pipelines.

Figure. Azure DevOps Variable Groups:

![Azure DevOps Variable Groups](/assets/note0006/azure-devops-variable-groups.png)