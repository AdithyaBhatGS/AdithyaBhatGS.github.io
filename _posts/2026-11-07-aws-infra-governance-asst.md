---
layout: post
title: "AWS Infrastructure Governance Assistant: Drift detection, Change History & Idle Resource Discovery"
date: 2026-11-07
categories: [Projects]
tags: [portfolio, aws, cicd, golden images, iac, drift-detection, policy-as-code]
pin: false
math: false
mermaid: true
---

> **TL;DR:** An engineering deep dive into personal portfolio project: *AWS Infra Governance Assistant* including key capabilities, design choices, limitations and possible enhancements.

---

![Dashboard](/assets/images/2026-11-07-aws-infra-governance-asst/dashboard.png)

## The Context & The Problem

### Problem 1

- In modern engineering environments the infrastructure is provisioned using the principle of **IAC** (Infrastructure As Code) which provides declarative capabilities to define, create, modify, and destroy the infra.

- When an organization provisions infra using **IAC** there is a possibility of accidental (manual) modifications to the maintained, provisioned infra.

- Whenever the infra is modified manually, the desired state will not match with the current state, which in turn is referred as *Drift*.

- **DevOps Team**, **Infra Team**, **Platform Team**, **Developers Team** need a way to detect this in a simple way rather than invoking custom commands or scripts or going to each stack in the UI and perform detect drift check (specifically for AWS CloudFormation Stacks).

- Now what if they have 20-30 stacks?

- Going to each stack and checking the drifts is time consuming, complex. Also there is a possibility that one might forget to check few stacks in between when there exists 20-30 stacks which can have significant consequence.

### Solution

- **AWS Infra Governance Assistant** solves this by providing a clean way to check infra drifts across your workloads through a **simple dashboard**.It not only provides *Drift Detection* at the **stack level** but at the **account level**.

- One can view each drift with the property differences across every stack in that account just by the click of the button in a single dashboard.

- Currently the drifts are detected for *AWS CloudFormation* workloads.

### Question

> If one can directly check drifts using *AWS Console*,*AWS CLI*, why do we even need a duplicate solution?

### Current Method

We have the options to detect resource drifts at a **stack level** without any cost, custom deployment complexities.

One can go to the *AWS Console*, click on a specific stack and click the detect drift to check the drift at the **stack level**.

Also to give you a simple overview, if one prefers drift detection through *AWS CLI* everytime they need to run:

**Step 1: Start a detection job**

```bash
aws cloudformation detect-stack-drift --stack-name <stack-name>
```

**Step 2: Monitor progress**

```bash
aws cloudformation describe-stack-drift-detection-status --stack-drift-detection-id <id>
```

**Step 3: Detect property differences**

```bash
aws cloudformation describe-stack-resource-drifts --stack-name <stack-name>
```

> Note: One should execute this every time per stack to detect the stack drifts.

### Enhancement being provided

However the project is not only providing the *Drift Detection* capabilities which is already provided by *AWS Console*, but it provides *Drift Detection* at the **entire account level** (not at the **stack level**).

Without this one might want to write custom scripts, manually go to the stack, click on the detect drift option which can be time consuming as an organization can manage 100s of stacks.

> A sample illustration:

![Account Level Drift Details](/assets/images/2026-11-07-aws-infra-governance-asst/account_drift_1.png)

![Drift Details Per Stack](/assets/images/2026-11-07-aws-infra-governance-asst/account_drift_2.png)

### Problem 2

- What if along with current *Drift Details*, the team requires the historical details?

- How many *Drifts* are being *added*, *removed*, *changed* each day since past 'n' days? Or per say need historical *Drift* details including:
    - *added*, *what drifts are added compared to previous drift detection scan*
    - *removed*, *what drifts are removed compared to previous drift detection scan*
    - *changed*, *what drifts are modified compared to previous drift detection scan*

### Solution

The project contains **Drift History** page which provides these capabilities so that one can easily check their infra healhy history.

> A sample illustration:

![Drift History 1](/assets/images/2026-11-07-aws-infra-governance-asst/drift_history_1.png)

![Drift History 2](/assets/images/2026-11-07-aws-infra-governance-asst/drift_history_2.png)

### Problem 3

- In organizations **idle resources** are one of the biggest issues as they can increase the cloud bills by significant amount.

- What if the organization have idle *Servers, EIPs, EBS volumes* but are not aware of them?

- What if they have many **NAT Gateways, Load Balancers** among which few of them are not needed?

### Solution

- The project provides a simple dashboard providing the information about:
    1. Unused Resource: EC2, EIPs, EBS volumes
    2. Cost Optimization: NAT Gateways, Load Balancers

- Here the thing to remeber is currently the *Cost Optimization* category represents resource which are not necessarily idle but their presence itself can have huge imapact. So need a way to just list each and every resource (NAT,ELBs), provide warning regarding them.

> A sample illustration:

![Resource Discovery](/assets/images/2026-11-07-aws-infra-governance-asst/resource_discovery.png)

Not Finished Yet...
Hope you find this meaningful,\
Thank you 🙂...

---