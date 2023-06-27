---
title: "GSoC Week1: Migrating assets to AWS S3" 
date: 2023-06-04
tags: ["gsoc"]
series: ["gsoc"]
featured: false 
---

# Week 1 (29 May - 4 June)

The Community bonding period was an incredible and refershing experience overall. I was excited and eager to start coding after the mentor meets. As a result I started
coding few days before the actual period. This additional time was beneficial since the first task involves important production migration. Such tasks are not
very straightforward, so the extra time allowed me thoroughly validate and test each part of my code.

Although there aren't many models with image attachments to migrate, the existing ones use different gems for handling attachments, namely CarrierWave and Paperclip.
Consequently, the configurations are differ for each.

My approach involves first mirroring the attachments to ensure that all new data goes to both the configurations - (paperclip & ActiveStorage) and (CarrierWave & ActiveStorage). Subsequently, we will backfill all the previous data. Once it is validated that all attachments have migrated correctly then we can update the code to 
serve assets with ActiveStorage.

This will hence take two deploys in order to ensure that there is no downtime and users can create circuits on [CircuitVerse](https://circuitverse.org/) 
without any issues.

So far the journey has been remarkable, I have learned a great deal. My main focus on this task is to ensure:

- Zero downtime
- Avoid duplicate attachments
- Successful migration of all previous data.

PR for the task - https://github.com/CircuitVerse/CircuitVerse/pull/3771

I will soon be posting a detailed blog discussing the technical aspect of my work. Stay tuned for that!
