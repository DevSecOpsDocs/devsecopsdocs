---
title: "TFSec"
description: "Minimum Viable DevSecOps 1.2 Self Assessments."
lead: "Self assessments performed by development, security, and operations teams provides them with the knowledge they need to improve."
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "rego"
weight: 323
toc: true
---

The [tfsec](https://github.com/aquasecurity/tfsec) utility is an open source static code analysis tool for Terraform maintained by Aqua Security. It checks for misconfigurations in most major cloud providers, hundreds of built in rules, and is built on top of rego policies. 

### Installation

To install tfsec you can refer to their [installation documentation](https://github.com/aquasecurity/tfsec#installation) as well as manually install the [releases](https://github.com/aquasecurity/tfsec/releases). 

### Usage

Getting started with tfsec is as simple as `tfsec .` to scan the current directory recursively. TFSec will immediately begin scanning your IaC and giving you recommendations for securing your infrastructure. 

### Findings

#### DynamoDB Encryption

```
Result #1 HIGH Table encryption is not enabled.
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  remotestate/main.tf:38-55
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   38  ┌ resource "aws_dynamodb_table" "dynamodb_table" {
   39  │     name           = var.table_name
   40  │     billing_mode   = "PAY_PER_REQUEST"
   41  │     hash_key       = "LockID"
   42  │     read_capacity  = 0
   43  │     write_capacity = 0
   44  │
   45  │     attribute {
   46  └         name = "LockID"
   ..
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
          ID aws-dynamodb-enable-at-rest-encryption
      Impact Data can be freely read if compromised
  Resolution Enable encryption at rest for DAX Cluster

  More Information
  - https://aquasecurity.github.io/tfsec/latest/checks/aws/dynamodb/enable-at-rest-encryption/
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dax_cluster#server_side_encryption
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

#### CloudFront 

This finding identifies a weak minimum TLS policy applied to the CloudFront distribution. In which TFSec recommends `minimum_protocol_version = "TLSv1.2_2021"` through the provided [check recommendation](https://aquasecurity.github.io/tfsec/latest/checks/aws/cloudfront/use-secure-tls-policy/) page.

```

Result #3 HIGH Distribution allows unencrypted communications.
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  s3_static_site/main.tf:63
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    5    resource "aws_cloudfront_distribution" "distribution" {
    .
   63  [     minimum_protocol_version = "TLSv1.2_2019" ("TLSv1.2_2019")
   ..
   67    }
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
          ID aws-cloudfront-use-secure-tls-policy
      Impact Outdated SSL policies increase exposure to known vulnerabilities
  Resolution Use the most modern TLS/SSL policies available

  More Information
  - https://aquasecurity.github.io/tfsec/latest/checks/aws/cloudfront/use-secure-tls-policy/
  - https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_distribution#minimum_protocol_version
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```