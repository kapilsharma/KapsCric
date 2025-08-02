# KapsCric

KAPsCric is the example project, for me to practice some of the programming language and frameworks. Its code is available for everyone to review.

This repository is just documentation to list the business requirements, SRS, Technical Documentation, etc. We will implement these requirements in separate repositories, in different programming languages and frameworks.

The goal is, project should be big enough to properly practie everything but not too big so that we repeat the concepts a lot and waste lot of time.

Also, these repositories will showcase whole project process from initial requirements to final release & ongoing management. We will discuss tech stuff and server (Azure/AWS/GCP) release process with Dev-Ops, CI/CD pipelines and IaC.

## Customer request.

We need to make a clone of ESPNCricInfo. It should have following features

- List down upcoming matches By `Role: admin`
  - `Role: visitor` should be able to view upcoming matches
- User management
  - `Role: superadmin` will be created in DB. He should be able to create
    - `Role: admin` - Able to list down the matches can add users as `Role: Commentator`
    - Assign match to one or more `Role: commentator`, so that they can do commentery on the match.
- Match page
  - Have following tab on match page
    - Live commentery
    - Scoreboard
  - Once the match start `Role: Commentator` should be able to login and do ball by ball commentery
  - `Role: visitor` should be able to watch current matches.
  - `Role: visitor` can sign up/login to become `Role: user`.
  - `Role: user` can add comments at any delivery
    - Those comments must be approved or replied by `Role: Commentator` to be visible on commentery.

These are the initial requirements given by the client. Obviously they are not enough for Business Analyst/Product Owner, to comeup with Software Requirements Specifications (SRS). He/She should comeup with followup questions to be discussed over email/call.

Let's assume clients are not available for calls, and all communication happens over the email/chat, so that we can list it here. 

## Initial mails

With above requirements, let's say our Product owner have following mail chain with the customers. Assume customer name is Mac and product owner name is Prince