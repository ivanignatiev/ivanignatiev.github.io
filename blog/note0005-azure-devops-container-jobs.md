---
slug: azure-devops-container-jobs.md
title: 'Note 5: Boost Python Pipelines Speed by 3-5x'
authors: ivanignatiev
tags: []
date: 2025-03-28T09:30
---

There's no need to install Python dependencies for tests every single time with Microsoft-hosted agents for Azure Pipelines.

Instead, run your tests inside containers that have everything pre-installed and preconfigured. This way, the dependency installation time is replaced by the quicker process of pulling a container image.

Figure. From dependencies installation in pipeline to using container job.

![Boost Python Pipelines Speed by 3-5x](/assets/note0005/faster-pipelines.svg)

:::tip Special tip for ModelOps and MLOps: 

When running unit tests, install the CPU-only version of PyTorch. This significantly reduces the image size and accelerates the container pull time.

:::


## Configure container Jobs in Azure DevOps

In addition to `pool:` job setting add container configuration:

```yaml
pool:
    vmImage: ubuntu-latest

container:
    image: "{reference to an image with dependencies installed}"
    endpoint: "{registry Azure DevOps project service connection}"
```

## Install PyTorch CPU only

It will not only accelerate container image building but also it will reduce the size of the container image.

```sh
pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cpu
```

## References

- [Container jobs in YAML pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/container-phases)
- [PyTorch Installation Commands](https://pytorch.org/get-started/previous-versions/)