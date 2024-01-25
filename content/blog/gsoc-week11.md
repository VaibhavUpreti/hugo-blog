---
title: "GSoC Week 11"
date: 2023-08-13
tags: ["gsoc"]
series: ["gsoc"]
featured: false
---

# Week 11 (14 August - 20 August)

As the Google Summer of Code (GSoC) program draws to a close, I find myself reflecting on the rewarding journey of the past few weeks.
My primary focus during this period was to wrap up pending tasks and to document added features.

One of the main tasks for this concluding week was dedicated to documentation.
I took the time to thoroughly document the changes made throughout the GSoC period, with a particular emphasis on detailing the integration of OpenTelemetry. 
This involved providing comprehensive instructions on how to utilize OpenTelemetry locally, ensuring that users can seamlessly implement and 
make the most of this powerful tool.


Explore the OpenTelemetry architecture through a visual representation.

![otel-arch.png](/images/gsoc/otel-arch.png)

This week, I had some crucial discussions with my mentor Aboobacker, focused on transitioning from the infrastructure of the application from EC2 to Mrsk. 
We explored intricacies and opted for a Blue-Green deployment for a seamless shift, ensuring minimal disruption and thorough testing.
Aboobacker has been incredibly helpful throughout the GSoC period. I've also learned a lot from him during this time, gaining valuable knowledge.

**Pull Requests:**

- [docs: distributed tracing using OpenTelemetry #5](https://github.com/CircuitVerse/infra/pull/5)
- [feat: distributed tracing using OpenTelemetry #3947](https://github.com/CircuitVerse/CircuitVerse/pull/3947)
- [feat: continuous deployment workflow using github actions and mrsk #3952](https://github.com/CircuitVerse/CircuitVerse/pull/3952)
