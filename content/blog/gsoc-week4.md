---
title: "GSoC Week 4: Distributed Tracing and Runbook Development"
date: 2023-06-25
tags: ["gsoc"]
series: ["gsoc"]
featured: false
---

## OpenTelemetry

For week 4 I spent most of my time working with Opentelemetry configuration for CircuitVerse.

[OpenTelemetry](https://opentelemetry.io/) is an observability framework and toolkit designed to create and manage
telemetry data such as

- traces
- metrics and
- logs

OpenTelemetry can be used with a variety of backends, including open source
tools like Jaeger and Prometheus, as well as commercial offerings like New Relic OpenTelemetry.

For Instrumentating a Rails app we would have to use opentelemetry-ruby sdk to create telemetry data. The sdk is a WIP and
currently only supports instrumentating traces using http protocol.
Therefore, exporting metric and log telemetry data is not currently possible

For a detailed guide to distributed tracing check refer to: [distributed-tracing-opentelemetry](/blog/distributed-tracing-opentelemetry)

## Runbooks

In addition to my work on OpenTelemetry (Otel), I dedicated time to creating runbooks for CircuitVerse.
Runbooks are comprehensive guides that outline important production deployment workflows and procedures.
These runbooks are essential because they cover crucial aspects that are not documented anywhere else.

PR - [initialise runbooks](https://github.com/CircuitVerse/infra/pull/3)

## Circuit Image Preview Migration

This week we also concluded with the migrations of assets.
We wrapped up the image preview migration work this week. The next step is push these changes to production in a pair programming session
with my mentor.

PR - [Migrate Image Preview to S3 (#3813)](https://github.com/CircuitVerse/CircuitVerse/pull/3813)

## Whats Next?

After a detailed discussion with my mentor, he advised me to focus on the migration tasks and the continuous deployment part
first while considering the remaining project goals as secondary.
Since these tasks require additional RAM usage, it is advisable to incorporate them once we have migrated to object storage and
established a stable continuous deployment workflow. Hence we will be integrating the monit configuration and OTEL setup at a later stage.

Honestly, I really enjoyed diving deep into server management and monitoring using Opentelemetry.
This experience taught me a great deal about handling production deployments
