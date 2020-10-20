
+++
title = "Inline Scanning"
chapter = false
weight = 04
+++

**TrainingNote** Inline scanning detailed description here

There are two general approaches to scanning images in Sysdig - backend scanning or inline scanning.  

Backend scanning requires that images be transferred to the Sysdig Backend to be scanned and evaluated.  With this method Sysdig must access your image repository and pull down the actual image to scan it. This requires Sysdig to store the credentials of your repository. This be problematic in a secure environment and/or when you're using SaaS.

However, with Inline Scanning the scan and subsequent analysis occur where the image resides, maybe as part of a CI/CD pipeline, or within your cloud environment, for example with ECR. In this case Sysdig down not need access to your repository and only the metadata is sent back to the Sysdig backend, hence no registry keys are exposed to Sysdig.

The benefits of scanning inline include
 - Images don't leave their own environment
 - SaaS users don't send images and proprietary code to Sysdig's SaaS service
 - Registries don't have to be exposed
 - Images can be scanned in parallel more easily
 - Images can be scanned before they hit the registry, which can cut down on registry costs and simplify the build pipeline
 - Existing scan metadata can be checked against new vulnerabilities, so images do no need to be rescanned, for example upon detection of zero-day vulnerabilities



## Technical Discussion

With Sysdig, there are two stages in scanning an image

1. Analysis of Contents (i.e. Bill of Materials)
2. Evaluation against policies and vulnerabilities matching

### Bill of Materials


#### Evaluation
