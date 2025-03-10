---
title: "Hitchhiker's Guide to GSoC: Week 2, Don't Panic Edition"
date: 2023-06-11
tags: ["gsoc"]
series: ["gsoc"]
featured: false
---

It was an exhausting week filled with challenges, unexpected workload from college was overwhelming, leaving me with an
awful lot of work to handle. On top of that, I had to juggle with my extracurricular commitments as well. It was a constant struggle to stay on top of everything and
find a balance between my various responsibilities. I did manage to make good progress on my GSoC tasks, although I acknowledge that I still need to improve my
time management skills.

Coming to the technical part, lets see...

#### AWS S3 Migration Task

The main plan for this task has been completed, and most of the work is done. Now it's a waiting game for all assets to migrate to S3.
Once the server migrations for asset migration are executed successfully, we will update the models, views and controllers to serve assets with ActiveStorage, for
which I have all the changes available and am done with that work. I have also resolved some CI errors on the previous PR and now it is now ready to be merged.

Final PR - https://github.com/CircuitVerse/CircuitVerse/pull/3786

#### Collaborating with Fellow Student

GSoC is all about open source, collaboration and community Growth. During the first two weeks I helped Arnab, another GSoC'23 student at CircuitVerse, with the
production configurations for imporving the Vue JS simulator simulator to the main repository. This involved reviewing Arnab's code, trying to deploy his code on
my personal CircuitVerse deployment, assisting with production deployment configurations, and providing valuable insights and inputs for the project.

#### Working on Other Smaller tasks along with AWS assets migration

My GSoC mentor had recommended from the beginning that I work on some smaller tasks alongside the Migration task. Since the Migration task was quite lengthy and time
consuming, I took his advise and worked on the following tasks in parallel:

##### Monit

[Monit](https://mmonit.com/monit/) is an open source server monitoring tool, it conducts automatic maintenance and repair and can execute meaningful tasks.
This week I worked with finalising the Monit Configs. for CircuitVerse, also setting up SMTP service to receive email alerts.

Added configurations for Monitoring following services:

- Server Resources
- Ping HTTP PORTs to check response
- Redis
- Postgresql
- Puma
- Sidekiq
- Caddy Webserver

However, after some thinking I have decided to remove monitoring sidekiq and puma individually, instead monitor [procodile](https://github.com/adamcooke/procodile) instead. Since the puma and sidekiq threads are handled by this tool, it is redudant to monitor these individually.

This is almost ready, after review from mentor, we can possibly implement these changes in a pair programming changes on the server.

Monit PR - https://github.com/CircuitVerse/infra/pull/1

##### Opentelemetry

I spent my time setting up New Relic Agent on my personal CircuitVerse deployment server, also adding configurations to export Opentelemetry data (traces and metrics),
from my Rails app(CircuitVerse) to New Relic following their [best practices](https://docs.newrelic.com/docs/more-integrations/open-source-telemetry-integrations/opentelemetry/best-practices/opentelemetry-best-practices-overview/) guide.

Here how we see the Traces that are exported from the OTEL collector(which is a docker container running on the server), I am quite impressed with the New Relic agent,
its certainly a tough competition to Jaegar which leads in being the best open source tool for Distributed Tracing using OTLP(Opentelemetry protocol).

Most probably this task will be implemented on the server after the resource intensive task of migrating assets to S3 has been completed, since it involves running and
extra docker container.

![newrelic-dashboard](/images/new-relic-otel-dashboard.png)

#### Apart From GSoC

Apart from GSoC, I spent my time studying for College exams and preparing for AWS Cloud certifications. I am targetting an associate level certificate, probably
AWS Certified Solutions Architect, although AWS Certified Developer also seems tempting. I have been procrastinating this for months now, largely due to my habit of piling unncecassary work.

Overall I need to get better at Time management and need to prioritize the right tasks earlier on. GSoC or in general working with open source software
has certainly had a positive impact on my life, both professionally and personally.
