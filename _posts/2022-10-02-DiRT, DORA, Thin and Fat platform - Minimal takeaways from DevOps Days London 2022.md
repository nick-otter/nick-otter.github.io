---
layout: post
title:  "DiRT, DORA, Thin and Fat platform - Minimal takeaways from DevOps Days London 2022"
categories: misc
---

# DiRT, DORA, Thin and Fat platform - Minimal takeaways from DevOps Days London 2022
{: style="text-align: center"}

Written by Nick Otter. 

# Intro

We (8x8) attended DevOps Days London 2022 this week. Here are my personal takeaways. For fun and laziness - I am only listing each takeaway as a one liner, to be explored in more depth outside of this post.

# Platform as a Product

* Use PACT (code-first consumer-driven contract testing tool) to test Infrastructure.
* SLA contracts for platform changes that have to pass Dev tests before deployment.
* Platform resources are not considered implicit, there is a feature roadmap.
* User personas for Dev teams, try and group dev needs as user personas.
* Platform repos are completely open to Dev, innersource mentality - dev can PR immediate changes they need if there is too much of a backlog of work.
* Understand Thin platform team or Fat platform team.

# Issues with Embedded DevOps
* Dedicated DevOps per Dev team creates Siloing without tribal knowledge unifying all DevOps individuals embedded in Dev teams.

# DevOps transfomations
* Evangelise Operations orchestrating DevOps not Dev movement to DevOps which squeezes out Operations.
* Understand Operations DevOps vs. Cloud Engineer/Platform engineer vs. Embedded DevOps.

# Working past 'DevOps' as a job title
* DevOps as a job title is far too tenuous.
* 'DevOps engineer' may mean working as Operations with some dev tooling.
* 'DevOps engineer' may mean acting as an informal platform architect.
* 'DevOps engineer' may mean being a Cloud Engineer.

# DevOps metrics of success
* DORA.
* SLA contracts for platform changes that have to pass Dev tests before deployment.
* CALMS - Culture, Automation, Lean, Measurement, and Sharing.
* DiRT - Disaster Recovery Test, regular firedrills.

# Terraform
* Be wary of Terraform module dependencies.

# Incidents
* Weekly firedrills, DiRT weekly type events.
* Service can't go live without thorough documentation and incidence response steps.
* Documentation should pass the '3am test'.
* Diverse pool of engineers on call at the same time, primary can escalate to SME.
* Who can fix it in 10mins? Don't chase your losses.

# Misc
* 'Centralised platform architecture to collaborative platform architecture'
* 'Domain based boundaries for services'
* 'CUPID idiomatic vs. SOLID' 
* 'An architectural quantum'
* Using Pulumi, single language, single framework with Dev
* Application driven systems, decoupled from cloud provider
* Feature management DevOps.
* Team topologies.
* Flow aligned, complex subsystems.
* Keep deployment configuration in Dev codebase as much as possible
* 'Open source license auditor, audits most dependended upon open source projects'

---

Thanks. This was written by Nick Otter.
