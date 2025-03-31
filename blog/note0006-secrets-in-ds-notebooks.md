---
slug: secrets-in-ds-notebooks
title: 'Note 6: Manage secrets in different notebooks environments'
authors: ivanignatiev
tags: []
date: 2025-03-29T09:30
---

Look to tutorials for Data Engineers and Data Scientists, responses in Stackoverflow. Hardcoded credentials ([CWE-798](https://cwe.mitre.org/data/definitions/798.html), [CWE-259](https://cwe.mitre.org/data/definitions/259.html)) are everywhere, if not in code, printed in output cell. Risk to push notebook with api-keys to git or a pipeline is very high.

Static code analysis tools:

- `pynblint` does not detect hardcoded secrets in notebook
- `bandit` has limited capabilities and does not fully support `.ipynb` notebooks

:::note
   I have tested bandit 1.8.3. The same code in `.py` and `.ipynb` cell. The variable with name `API_KEY` and hardcoded value is not detected, `password` variable is detected only in `.py` file ([B105: hardcoded_password_string](https://bandit.readthedocs.io/en/latest/plugins/b105_hardcoded_password_string.html#b105-hardcoded-password-string)).
:::

Anyway, the challenge for Jupyter Notebooks is possibility to have multiple natures of code in the same notebook, so Python tools are not enough:

- `gitleaks`
- `detect-secrets` has many false positives and need to be fine-tuned but it looks in the cell's code and output

More tools described in [OWASP Security Tools - Secrets Detection Tools section](https://owasp.org/www-community/Free_for_Open_Source_Application_Security_Tools).

Issues can be caught during code review. However, file will be distributed across all team members PCs if merged in some shared branch.

Public services like GitHub and some serverless services ([Scanning AWS Lambda functions with Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/scanning-lambda.html)) can identify [CWE-798](https://cwe.mitre.org/data/definitions/798.html) but it does not reduce responsibilities of notebooks and code owners.

## Local Notebooks

Use environment variables, or `dotenv`, or `keyring`, or password inputs (i.e. `getpass`) for passwords and api keys.

Environment variables example:

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

`dbutils.secrets.get(scope, key)` for Python:

```python
dbutils.secrets.list('{databricksSecretNamespace}')
username = dbutils.secrets.get(scope = "{databricksSecretNamespace}", key = "{databricksSecretName}")
```

`secret(scope, key)` for SQL queries:

```sql
SELECT * FROM list_secrets();
SELECT secret('{databricksSecretNamespace}', '{databricksSecretName}');
```

- [Tutorial: Create and use a Databricks secret](https://learn.microsoft.com/en-us/azure/databricks/security/secrets/example-secret-workflow)
- [secret function](https://learn.microsoft.com/en-us/azure/databricks/sql/language-manual/functions/secret)

## Azure DevOps

In MLOps, ModelOps in pipelines secrets can be stored over Variables Groups. Additionally, Variable Group can be linked to Azure KeyVault.

From Variables secrets can be populated as environment variables in pipelines.

Figure. Azure DevOps Variable Groups:

![Azure DevOps Variable Groups](/assets/note0006/azure-devops-variable-groups.png)