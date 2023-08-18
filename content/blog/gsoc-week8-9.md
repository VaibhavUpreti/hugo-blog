---
title: "GSoC Adventures: Week 8 & 9"
date: 2023-07-30
tags: ["gsoc"]
series: ["gsoc"]
featured: false 
---

# Week 8 & 9 (24 July - 6 August) 💻

The past two weeks have taken me on an exhilarating journey as I immersed myself in the heart of the project. So, let's put on our virtual hardhats and delve into the captivating events of Week 8 and 9.

I was on fire with coding, experimenting extensively with the new tool [mrsk.dev](https://mrsk.dev/).
 I dabbled in different configurations and set up the groundwork for our fresh deployment method. My guiding principle was to ensure my changes were airtight against security vulnerabilities throughout this period.

This included trying various configurations, setting up the infrastructure for the new deployment method and many other tasks.

During this time, I dedicated myself to about 90 commits on my test branch, creating a staging environment to assess my alterations.

A substantial chunk of my effort was invested in refining configurations. Regular discussions with my mentor fueled my progress, and thorough testing became my mantra.

This endeavor led me to establish a continuous deployment pipeline. Which with every repository commit triggers the creation of a CircuitVerse Docker image, 
which is then pushed to Docker Hub and subjected to a series of checks.

With these checks greenlit, I crafted a deploy job that elegantly transports the latest image to the server, minimizing any potential downtime.(which is done by mrsk)

Of course, not everything was smooth sailing. Tackling quirks, particularly in the realm of arm64 and amd64 builder ruby images, tested my patience.
But embracing challenges as disguised opportunities, I tenaciously tackled these obstacles.


# What's Next?

Currently simmering on the horizon is a technical blog post. In it, I'll guide you through the ins and outs of deploying CircuitVerse or any application using mrsk.
Furthermore, I'll unveil the magic of automating the build process via GitHub Actions, promising uninterrupted deployments.

Stay with me for more updates. The adventure continues!

> [Commits](https://github.com/vaibhavupreti/CircuitVerse/tree/main)
