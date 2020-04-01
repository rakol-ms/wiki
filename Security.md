# Security review request

This template will help guide Security on what further reviews may be required for your project. Please elaborate beyond Yes and No answers if you feel like the additional context will help clarify the risk of your project.

After we receive this template, teams across Security will review the responses and follow up with the next steps required by their specific teams.

## General information
- **Project name:**  
    - Container Scanning 
- **Project tracking issue / project board / repo:**
  - https://github.com/Azure/container-scan
- **Project background:**
  - Container scanning and container policy enforcement are critical for any enterprise/federal customer in easing the hardening process. GitHub can help customers achieve their goal to secure containers and make GitHub the preferred destination for OSS projects publishing container images over time. The end goal for the same will be achieved in multiple phases. 
  
  **Phase 1** - As part of the first phase, user takes help of a new action defined by us and makes use of the same in a PR and CD flow for detecting and flagging vulnerabilities present in the container image. This part is already a public repo (https://github.com/Azure/container-scan) and is for customers to use.

    **Phase 2** - An option for the user to merge the vulnerabilities found as part of the scan to the existing whitelist will be provided  as part of this phase. The flow is as follows -
    
     The container scan action posts the result to a new service which in turn posts a checkRun on to the dotcom. The checkrun would contain a button for the user to opt for merging the vulnerabilities to the existing whitelist file. A new github app is registered with a webhook for listening on the buttonClicked event. This notification also ends up in the same service. The service in turn edits the PR as appropriate and merges it back.

    The call flow would be as follows:
         Workflow is run with the container scanning action
         Action -> Service
         Service -> Github (for auth)
         Service -> Github ( Post CheckRun with vulnerabilities, with action button)
         User Clicks the Action Button( Part of earlier Check Run)
         Lands up in the service as part of Github App event handler
         Service Edits the PR and commits it back
- **GitHub point of contact for Security:**
- **GitHub engineering team owning the service/code:**
    - gh-container-crew 
- **GitHub [Service Catalog](https://catalog.githubapp.com/services) Entry:**
- **Planned ship date(s):**
  -  Phase1 is in the process of getting shipped
  -  Phase2 is supposed to be shipped by April 3rd Week
 - **Product release plan:**
    - This is supposed be demonstrated in the Github Satellite 2020
  ## Questions for Security
- What are specific areas that you are concerned about from a security perspective and would like to discuss in depth?
 - Action to service interaction
    - Proposal( Run service in Azure) - To run the scan service(new) in Azure with a public end point that will be invoked from the action. The service will invoke github to check if the token is valid as part of authorization. 
    - Alternative Proposal( Run as a Moda Service) - In this approach we run the service as a Moda service and use dotcom as a proxy to route the calls rom action to "scan service". This looks to have changes to the dotcom code.
    - 
- Service backing the  Github App
    - The service will back the Github App 

## Scoping details 
- **Will it be a new or change to an existing GitHub.com feature?** 
  - New
- **Will you be writing new code for a production service? If so, what repo hosts this code?**
  - https://github.com/actions/checkout
- **Who are the users of the new feature/product/function?** 
  - general users
- **How will it be accessed?**
  - GitHub Actions
- **Does it create or modify content, or change how content is shared?**
  - No
- **What type of data does it store, process, and/or transmit?**
  - User's private SSH key
- **What 3rd party services or vendors will be used, if any?**
  - n/a
- **What new software/libraries/daemons/etc will be used, if any?**
- **What changes will be required to our production infrastructure, if any?**
- **Where will this service be hosted?** 
  - _Help: Internal - VPC, Moda, GC2, GitHub Datacenter; Third Party - Heroku, External AWS, GCP, Azure_
- **Will this service be accessible external to GitHub’s infrastructure?**
- **Will this change impact any SOC 2, FedRAMP, PCI, or other compliance controls?**
   - [ ] Yes - I will outline the impacted controls in a [Security-GRC issue](https://github.com/github/security-grc/issues/new/choose)
   - [ ] No - I am confident this impacts no audited control (SOC, FedRAMP, PCI, Privacy) or sensitive data (PCI, PII, HIPAA, IP).
   - [ ] I do not know. I will open an issue for assistance from [Security-GRC](https://github.com/github/security-grc/issues/new/choose)

You made it :tada: /cc @github/security-reviewers to check this out!

## Review status (_completed by each security team_)
- [ ] AppSec Review - (_link to follow up review_)
- [ ] Platform Health Review - (_link to follow up review_)
- [ ] SecOps Review - (_link to follow up review_)
- [ ] Security-GRC Risk Review - (_link to follow up review_)
- [ ] Security-GRC Compliance (@davidwaltonhall, @SarahFongheiser, @ebchill) - (_link to follow up review_) 
   *_Approval from Security-GRC Compliance is not required to proceed._
