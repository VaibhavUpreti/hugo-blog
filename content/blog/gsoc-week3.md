---
title: "GSoC Chronicles: Week 3"
date: 2023-06-18
tags: ["gsoc"]
series: ["gsoc"]
featured: true
---


Probably the best week of my GSoC journey so far. 

The changes that were merged this week will have a direct impact on of hundreds of thousands of users.
This is the first time I have pulled off such a task. I am thankful to my mentor Aboobacker for guiding me through it.

Despite numerous potential challenges, we effectively replicated attachments and seamlessly conducted data migrations,
ensuring  seamless transfer of previous data without any disruption or downtime.

Issue: [#3764](https://github.com/CircuitVerse/CircuitVerse/issues/3764)

PR: [mirror-attachments](https://github.com/CircuitVerse/CircuitVerse/pull/3786)


Currently we are waiting for the completion of object migrations.
Once all objects have been successfully migrated and pass through validations, we will be able to serve assets 
using ActiveStorage.


Having attachments on S3 will lead tosignificant liberation of storage space on our server infrastructure.
This step will thus offload the burden of managing and maintaining large volumes of data on our local servers
and will drastically reduce our infrastructure costs. Apart from reducing costs,it will provide operational efficiency
and flexibility to adapt and scale our infrastructure.


Apart from the migration task, I have completed the monit configuration

Pull: https://github.com/CircuitVerse/infra/pull/1

After initial review from my mentor, he advised me to transform this repostiory into a runbook covering basic workflows & operations for CircuitVerse, example [gitlab-runbook](
https://gitlab.com/gitlab-com/runbooks). 

I have to also raise a Pull request for the Opentelmetry task, after confirming that while running the data migrations
we aren't experiencing any bottlenecks in the memory we are good to integrate distributed tracing in CircuitVerse

Some new issues that arised this week:

1. Update redis on server
2. Setup CORS & allow direct uploads to ActiveStorage (after migrating all past data) [#3808](https://github.com/CircuitVerse/CircuitVerse/issues/3808)
3. Reduce size of large attachments & set a limit for uploading large attachments in the frontend [#3810](https://github.com/CircuitVerse/CircuitVerse/issues/3810)
4. Update Docker dependencies for ActiveStorage #[3809](https://github.com/CircuitVerse/CircuitVerse/issues/3809)
5. Updating procodile gem on server, using a stable version for monit start and stop commands to work.

I will now work one these in the coming week.
