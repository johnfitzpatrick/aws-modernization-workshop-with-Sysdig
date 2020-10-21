
+++
title = "Inline Scanning"
chapter = false
weight = 04
+++

There are two general approaches to scanning images in Sysdig - backend scanning or inline scanning.  The reasons why you might choose one over the other is best explained by an understanding of how scanning works under the hood.

![Inline Scanning](/images/00_introduction/inline_scanning01.png)

<!-- <img src=/images/00_introduction/inline_scanning01.png width="50%" height="50%"> -->

## Technical Discussion

With Sysdig, there are two phases in scanning an image

1. Analysis of Contents (creation of a Bill of Materials)
2. Evaluation against policies and vulnerabilities matching

### Phase 1 - Analysis of Contents

During the analysis phase of the scan, the worker first loads the image.  Each image is built upon a series of other images, called 'layers', each one referred to by specific SHA value in the manifest file. During the analysis each layer is pulled down and analysed and a complete list of all the files, packages, package versions, etc across all the layers is generated.

This phase of the scan is quite process-heavy and accounts for approximately 90% of the Time/CPU consumed during the entire scan, and the output of this is a JSON document containing metadata on all aspects of the image.

During an inline scan this phase happens in the image's environment - in a CI/CD pipeline, at by an Admission Controller, or even on the Kubernetes worker node.

#### Going A Little Deeper...

The analyser itself uses the Anchore engine and runs ~15-20 different analysers, each performing differnt functions - size of files, package versions, etc. Each analyser generates a JSON report containing metadata, all of which are then combined.

<details>
<summary>Expand here to view some some sample output to give you some visualisation of how this looks</summary>

    ```
    {
      "document": [
        {
          "image": {
            "imageId": "f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35",
            "imagedata": {
              "analysis_report": {
                "analyzer_meta": {
                  "analyzer_meta": {
                    "base": {
                      "DISTRO": "debian",
                      "DISTROVERS": "10",
                      "LIKEDISTRO": "debian"
                    }
                  }
                },
                "file_checksums": {
                  "files.md5sums": {
                    "base": {
                      "/bin": "DIRECTORY_OR_OTHER",
                      "/bin/bash": "4600132e6a7ae0d451566943a9e79736",
                      "/bin/cat": "44b8726219e0d2929e9150210bfbb544",
                      "/bin/chgrp": "2befb2d66eee50af3fd5eb0b30102841",
                      "/bin/chmod": "737ae4345da6e93c44fe9b11b12defe1",
                      "/bin/chown": "8680c8e619194af847c009a97fd4ebe2",
                      "/bin/cp": "d38d5be99452fb23cce11fc7756c1594",
                      "/bin/dash": "895aea5b87d9d6cbd73537a9b2d45cff",
                      "/bin/date": "b175b76c42bf04d764f3f5d7e4f3c69c",
                      "/bin/dd": "1f90de0a1b75febeda1936a1ed9e1066",
                      "/bin/df": "b50d93d2ab75977d129baf0078becb96",
                      "/bin/dir": "3c76bcda677ed3ff9901d6e770ebca3d",
                      "/bin/dmesg": "ea95ebcd2794014a5f933f7b6434e31c",
    ...
    ```
</details>

You can see each binary, file etc is specifically referenced, and the number next to each entry relates to its version etc.

The results of this analysis are combined into a single JSON analysis report, or metadata document, and it is this metadata document that is used to evaluate the scan against the defines security policy.

For a backend scan, this process all happens within Sysdig, however during an 'inline scan', this metadata is then forwarded to the Sysdig Backend for evaluation using the API - this is the reason you must supply the **Sysdig Secure API Token** later in this workshop!

<details>
<summary>Expand for details of the JSON metadata document Sent to the Sysdig Backend</summary>
```
[
 {
  "sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77e59e3a8e4914": {
   "docker.io/amazon/amazon-ecs-sample:latest": [
    {
     "detail": {},
     "last_evaluation": "2020-10-21T11:08:22Z",
     "policyId": "default",
     "status": "fail"
    }
   ]
  }
 }
]
Status is fail
Result Details:
[
 {
  "sha256:36c7b282abd0186e01419f2e58743e1bf635808231049bbc9d77e59e3a8e4914": {
   "docker.io/amazon/amazon-ecs-sample:latest": [
    {
     "detail": {
      "policy": {
       "blacklisted_images": [],
       "comment": "Default Sysdig policy bundle for new customers.",
       "id": "default",
       "mappings": [
        {
         "id": "mapping_1CI5tw3zxNL9b344sSsXBfth3dW",
         "image": {
          "type": "tag",
          "value": "*"
         },
         "name": "default",
         "policy_ids": [
          "default"
         ],
         "registry": "*",
         "repository": "*",
         "whitelist_ids": [
          "global"
         ]
        }
       ],
       "name": "Default Sysdig policy bundle",
       "policies": [
        {
         "comment": "System default policy",
         "id": "default",
         "name": "DefaultPolicy",
         "rules": [
          {
           "action": "WARN",
           "gate": "dockerfile",
           "id": "rule_1FlJOnK9qdRSRcTNrfz3IUZXbou",
           "params": [
            {
             "name": "instruction",
             "value": "HEALTHCHECK"
            },
            {
             "name": "check",
             "value": "not_exists"
            }
           ],
           "trigger": "instruction"
          },
          {
           "action": "WARN",
...
```
</details>


### Phase 2 - Evaluation

Once the Sysdig Backend retrieves the metadata it is checked against the assigned policy definitions as well as the vulnerability database.  A Sysdig Secure policy is a combination of rules about activities an enterprise wants to detect in an environment, the actions that should be taken if the policy rule is breached, and potentially the notifications that should be sent.  These policies are configured through the Sysdig UI, and may differ from image to image.

Individual policy rules may relate to

 - Open ports
 - File permissions
 - Exposed passwords
 - etc

A number of policies are delivered out-of-the-box and can be used as-is, duplicated, or edited as needed. These relate specifially to 'PCI' and 'NIST 800-190', as well as general Dockerfile best practices.  You can also create policies from scratch, using either predefined rules or creating custom rules.


## Inline Scanning vs Backend Scanning

Inline Scanning is considered best practice and the better approach to scanning over backend scanning. Backend scanning requires that images be transferred to the Sysdig Backend to be scanned and evaluated.  With this method Sysdig must access your image repository and pull down the actual image to scan it. This requires Sysdig to store the credentials of your repository. This be problematic in a secure environment and/or when you're using SaaS.

However, with Inline Scanning the scan and subsequent analysis occur where the image resides, maybe as part of a CI/CD pipeline, or within your cloud environment, for example with ECR. In this case Sysdig down not need access to your repository and only the metadata is sent back to the Sysdig backend, hence no registry keys are exposed to Sysdig.

The benefits of scanning inline include

 - Images don't leave their own environment
 - SaaS users don't send images and proprietary code to Sysdig's SaaS service
 - Registries don't have to be exposed
 - Images can be scanned in parallel more easily
 - Images can be scanned before they hit the registry, which can cut down on registry costs and simplify the build pipeline
 - Existing scan metadata can be checked against new vulnerabilities, so images do no need to be rescanned, for example upon detection of zero-day vulnerabilities
