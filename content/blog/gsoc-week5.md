---
title: "GSoC Week 5 at CircuitVerse.org"
date: 2023-07-02
tags: ["gsoc"]
series: ["gsoc"]
featured: true
---


# Week 5 (26 June,23 - 2 July,23)

During week 5, I devoted most my time to Mrsk & working on serving assets using ActiveStorage.

---

# Mrsk

[Mrsk](https://mrsk.dev/) is a tool that allows you to deploy docker containers to bare metal servers, (or cloud VMs)
using docker with zero downtime. Mrsk is still in aplha stage but is now in production at [37signals](https://37signals.com/). 

Before my GSoC coding period, I managed to explored this tool and deployed CircuitVerse using mrsk to a fresh EC2 instance.
Before the start of my Google Summer of Code (GSoC) coding period, I managed to explored this tool and deployed CircuitVerse
to a fresh EC2 instance using Mrsk.

Following video showcases the deployment:

{{< youtube KWJlWUfPWhw >}}

---

During this time, I connected with my mentor and we had detailed discussions, exchanging ideas and exploring different strategies for deploying CircuitVerse to a production environment using Mrsk.

One important topic we deliberated on was whether to maintain the existing configuration of having PostgreSQL and Redis on 
the server itself or to transition them to containers while persisting data using volumes. 

In conclusion,the decision involved considering factors such as scalability, performance, ease of maintenance and optimised resource consumption. 

I am currently working on a detailed technical blog that focuses on deploying a Rails app using Mrsk and managing services like 
Redis and Sidekiq on the same server. Stay tuned for the implementation with step-by-step instructions.

🎉 In addition, this week marked the completion of Circuit image_preview migration to AWS S3, currently on production at 
[circuitverse.org](https://circuitverse.org/) we are mirroing attachments to both old and new configuration. Now that 
we have backfilled all the data. Its time to serve them using ActiveStorage and remove the deprecated gems(paperclip and
carrrierwave).


##### Patches Merged

**CircuitVerse**

- [fix: use env[] instead of fetch (#3856)](https://github.com/CircuitVerse/CircuitVerse/pull/3856)
- [feat: migrate image_preview to AWS S3 (#3813)](https://github.com/CircuitVerse/CircuitVerse/pull/3813)


**Infra**

- [feat: monit config files (#1)](https://github.com/CircuitVerse/infra/pull/1)
- [feat: Intialise runbook (#3)](https://github.com/CircuitVerse/infra/pull/3)
