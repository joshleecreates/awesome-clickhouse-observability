# Awesome ClickHouse® Observability

A collection of awesome resources for ClickHouse-powered observability. Includes both commerial and FOSS tools, as well as many resources.

*Last Updated July 28, 2026*

## Contents

- [Observability Platforms](#observability-platforms)
- [Ecosystem Tools](#ecosystem-tools)
- [PromQL in ClickHouse](#promql-in-clickhouse)
- [Thought Leadership](#thought-leadership)
- [Talks & Webinars](#talks--webinars)
- [How-Tos & Guides](#how-tos--guides)
- [Benchmarks](#benchmarks)
- [Case Studies & User Stories](#case-studies--user-stories)

## Observability Platforms

**OSS** = open source you can self-host · **SaaS** = vendor-hosted · **BYOC** = vendor-managed in your own cloud account. Tags reflect the primary offering(s) and are best-effort.

| Platform | Model | Description |
|---|---|---|
| [Better Stack](https://betterstack.com/) | SaaS | Log management with ClickHouse-backed search, uptime monitoring, incident management, on-call, and SQL-compatible log queries. |
| [Chronosphere](https://chronosphere.io/) | SaaS | Structured logs and metrics platform built on ClickHouse for cost savings and query speed at scale. |
| [ClickStack](https://clickhouse.com/clickstack) | OSS, SaaS | ClickHouse's official open source observability stack (ClickHouse + HyperDX UI + OpenTelemetry Collector) unifying logs, metrics, traces, and session replay. |
| [Coroot](https://coroot.com/) | OSS, BYOC | Observability and APM tool with AI-powered root cause analysis, using ClickHouse for storage and eBPF for zero-config instrumentation. |
| [Dash0](https://www.dash0.com/) | SaaS | Resource-centric observability platform described as "Instana with less enterprise and more OpenTelemetry." |
| [GigaPipe (qryn)](https://gigapipe.com/) | OSS, SaaS | Polyglot observability warehouse (formerly qryn) and drop-in Grafana LGTMP alternative, storing logs, metrics, traces, and profiles in ClickHouse behind Loki, Prometheus, Tempo, Pyroscope, and OpenTelemetry-compatible APIs. |
| [Datadog + ClickHouse](https://www.datadoghq.com/blog/datadog-clickhouse-log-management/) | SaaS | Datadog's integration for storing and searching high-volume logs in ClickHouse. |
| [GitLab Opstrace](https://gitlab.com/gitlab-org/opstrace) | OSS | Open source error tracking and tracing project that selected ClickHouse as its observability storage. |
| [groundcover](https://groundcover.com/) | BYOC | eBPF-based full-stack observability platform that deploys entirely in your own cloud, storing logs, metrics, traces, and eBPF-captured data in a self-hosted ClickHouse backend. |
| [IBM Instana](https://www.ibm.com/products/instana) | SaaS | Enterprise APM with 100% unsampled tracing powered by ClickHouse and a proprietary TSDB for near-real-time alerting (also offers self-hosted). |
| [Jaeger](https://www.jaegertracing.io/) | OSS | CNCF distributed tracing platform (originally from Uber) that supports ClickHouse as an experimental storage backend behind a feature gate, alongside Cassandra, Elasticsearch, and OpenSearch. |
| [Last9](https://last9.io/) | SaaS, BYOC | Offers 100% unsampled ClickHouse-powered tracing plus a control plane that manages telemetry at runtime without redeployments. |
| [PostHog](https://posthog.com/) | OSS, SaaS | Product analytics suite with error tracking that automatically connects errors to sessions, recordings, and feature flags. |
| [Sentry](https://sentry.io/) | OSS, SaaS | Error tracking and metrics platform that uses ClickHouse as its storage layer. |
| [SigNoz](https://signoz.io/) | OSS, SaaS | OpenTelemetry-native open source Datadog alternative using a ClickHouse columnar datastore, with query builder, PromQL, and raw ClickHouse SQL. |
| [Uptrace](https://uptrace.dev/) | OSS, SaaS | OpenTelemetry APM for traces, metrics, and logs that stores data in ClickHouse to cut storage and improve query performance vs. Elasticsearch. |

*All information is provided on a best-effort basis. Contact @joshleecreates for corrections*


## Ecosystem Tools

| Link | Description |
|---|---|
| [ClickHouse plugin for Grafana](https://grafana.com/grafana/plugins/grafana-clickhouse-datasource/) | Official Grafana data source for ClickHouse, now shipping pre-built OpenTelemetry dashboards for logs, traces, and per-service deep dives. |
| [OTel Collector ClickHouse Exporter](https://clickhouse.com/docs/observability/integrating-opentelemetry) | The OTel Collector Contrib distro's ClickHouse exporter and filelog receiver — the core pipeline component for a ClickHouse-based solution. |
| [Altinity Kubernetes Operator for ClickHouse](https://github.com/Altinity/clickhouse-operator) | Open source operator managing ClickHouse deployment, upgrades, backups, and replication in Kubernetes. |


## PromQL in ClickHouse

ClickHouse is growing native Prometheus Query Language (PromQL) support so time series stored in a `TimeSeries` table can be queried with Prometheus' own dialect instead of (or alongside) SQL. Support is experimental and evolving — the specs and PRs below track where it is headed.

| Link | Description |
|---|---|
| [Support for PromQL (Issue #57545)](https://github.com/ClickHouse/ClickHouse/issues/57545) | Open tracking issue (since Dec 2023) requesting a PromQL interpreter, Prometheus-optimized table structures, and native Prometheus HTTP / Remote-Read API integration. |
| [Basic support for the PromQL dialect (PR #75036)](https://github.com/ClickHouse/ClickHouse/pull/75036) | vitlibar's merged PR (Aug 2025, shipped in 25.8) adding a `dialect='promql'` mode over a `TimeSeries` table, initially supporting the `rate`, `delta`, and `increase` functions. |
| [Support for the Prometheus HTTP Query API (Issue #89553)](https://github.com/ClickHouse/ClickHouse/issues/89553) | Feature request to expose Prometheus HTTP API endpoints (`/api/v1/query`, `/query_range`, `/labels`, `/label/<name>/values`, `/parse_query`, `/format_query`) so Grafana and other tools can talk PromQL to ClickHouse. |
| [Remote-write URL-based table routing (Issue #93831)](https://github.com/ClickHouse/ClickHouse/issues/93831) | Proposal to route Prometheus remote-write per URL rather than one static handler per `TimeSeries` table in `config.xml`, needed for multi-tenant deployments. |
| [`prometheusQuery` table function (docs)](https://clickhouse.com/docs/sql-reference/table-functions/prometheusQuery) | Evaluates a PromQL instant query against a `TimeSeries` table from within SQL. |
| [`prometheusQueryRange` table function (docs)](https://clickhouse.com/docs/sql-reference/table-functions/prometheusQueryRange) | Evaluates a PromQL range query (start/end/step) against a `TimeSeries` table; supports selectors, label matchers, offset/`@` modifiers, subqueries, and `histogram_quantile` over classic buckets. |
| [Alternative query languages (docs)](https://clickhouse.com/docs/guides/developer/alternative-query-languages) | Guide to running PromQL (and PRQL/Kusto) in clickhouse-client via `dialect='promql'` plus the `promql_table_name` setting. |
| [Prometheus protocols (docs)](https://clickhouse.com/docs/interfaces/prometheus) | Reference for ClickHouse's native Prometheus remote-write/read protocols and the experimental `TimeSeries` table engine (since 24.8) that PromQL queries read from. |
| [ClickHouse Release 25.8 (LTS)](https://clickhouse.com/blog/clickhouse-release-25-08) | Release post announcing the initial PromQL dialect support, with the `rate`, `delta`, and `increase` functions in the first cut. |
| [ClickHouse as a Prometheus Remote Write Backend (OneUptime)](https://oneuptime.com/blog/post/2026-03-31-clickhouse-prometheus-remote-write/view) | How-to for wiring the `TimeSeries` table engine up as remote-write storage for Prometheus metrics. |
| [ClickHouse as a Prometheus Long-Term Storage Backend (OneUptime)](https://oneuptime.com/blog/post/2026-03-31-clickhouse-prometheus-long-term-storage/view) | Using ClickHouse for long-term Prometheus retention via the remote-write/read protocols. |


## Thought Leadership

Ordered newest to oldest.

| Date | Link | Description |
|---|---|---|
| 2026-04 | [Our vision for the ClickHouse Grafana plugin (ClickHouse)](https://clickhouse.com/blog/grafana-plugin-vision) | Roadmap for out-of-the-box OTel and Kubernetes dashboards so teams go from ingestion to usable dashboards in minutes. |
| 2025-11 | [Beyond sampling: petabyte-scale logs without losing data (ClickHouse)](https://clickhouse.com/resources/engineering/managing-petabyte-scale-logs-without-sampling) | Argues sampling was a workaround for search-based architecture limits, using Anthropic's Claude 3/3.5 scaling as the case study. |
| 2025-11 | [Best Open Source Observability Solutions (ClickHouse)](https://clickhouse.com/resources/engineering/best-open-source-observability-solutions) | Critique of the "best-of-breed" LGTM siloed stack versus a unified columnar approach. |
| 2025-09 | [Best Open-Source Observability Platforms in 2026 (Parseable)](https://www.parseable.com/blog/ten-best-open-source-observability-platforms-2026) | Decision guide mapping platforms to team needs across telemetry signals and cost tolerance. |
| 2025-08 | [ClickStack: ClickHouse's New Observability Stack Unveiled (Dotan Horovits)](https://horovits.medium.com/clickstack-clickhouses-new-observability-stack-unveiled-73f129a179a3) | Interview with HyperDX co-founder Mike Shi on designing a UX that scales from point-and-click to full SQL. |
| 2025-06 | [Scaling Observability beyond 100 Petabytes (ClickHouse)](https://clickhouse.com/blog/scaling-observability-beyond-100pb-wide-events-replacing-otel) | How LogHouse grew to 100+ PB across nearly 500 trillion rows, and the architectural changes that embracing wide events required. |
| 2025-05 | [ClickStack: A High-Performance OSS Observability Stack (ClickHouse)](https://clickhouse.com/blog/clickstack-a-high-performance-oss-observability-stack-on-clickhouse) | Launch post arguing wide events break the "three pillars" model and eliminate stitching siloed telemetry. |
| 2025-05 | [ClickHouse: Breaking the Speed Limit for Observability (Dotan Horovits)](https://horovits.medium.com/clickhouse-breaking-the-speed-limit-for-observability-and-analytics-2004160b2f5e) | Dotan Horovits on how a Yandex web-analytics OLAP store became one of the hottest observability backends. |
| 2024-11 | [The evolution of SQL-based observability (ClickHouse)](https://clickhouse.com/blog/evolution-of-sql-based-observability-with-clickhouse) | Year-in-review on JSON support, OTel integration, and time series capabilities in SQL-based observability. |


## Talks & Webinars

Ordered newest to oldest.

| Date | Link | Description |
|---|---|---|
| 2026-04 | [The Future of Observability is Open: Iceberg + ClickHouse + OpenTelemetry (Altinity)](https://altinity.com/webinarspage/the-future-of-observability-is-open-combining-iceberg-clickhouse-and-opentelemetry) | Josh Lee argues for openness across instrumentation, pipelines, and storage, demoing an OTel + Iceberg + ClickHouse "real-time open lakehouse" that stores telemetry on S3 at ~12x lower cost. |
| 2026-01 | [Magical Mystery Tour: A Roundup of Observability Datastores (FOSDEM 2026)](https://altinity.com/events/fosdem-2026-talk-magical-mystery-tour-a-roundup-of-observability-datastores) | FOSDEM 2026 edition of Josh Lee's roundup of observability datastores and how to choose between them for a given system. |
| 2025-09 | [LogHouse — the story behind ClickHouse's own logging platform (SREday London 2025)](https://www.youtube.com/watch?v=8gJF7MnjFAo) | ClickHouse observability engineer Rory Crispin charts LogHouse's journey to 100+ PB and 500 trillion rows by embracing "wide events" and replacing the OTel pipeline with the custom SysEx exporter (20x volume, 90% less CPU). |
| 2025-06 | [Distributed Tracing with ClickHouse & OpenTelemetry (Altinity)](https://altinity.com/webinarspage/distributed-tracing-with-clickhouse-opentelemetry) | Josh Lee and Maciej Bak demo the OTel Collector ClickHouse exporter into Grafana, plus using ClickHouse's own OTel tracing to observe itself. |
| 2025-03 | [Unified Observability: ClickHouse as a Comprehensive Telemetry Database (FOSSASIA 2025)](https://altinity.com/events/fossasia-summit-2025-unified-observability-leveraging-clickhouse-as-a-comprehensive-telemetry-database) | Conference talk on using a single ClickHouse database to unify logs, metrics, and traces. |
| 2025-02 | [O11y-in-One: Exploring ClickHouse as a Unified Telemetry Database (FOSDEM 2025)](https://archive.fosdem.org/2025/schedule/event/fosdem-2025-5960-o11y-in-one-exploring-a-unified-telemetry-database/) | Josh Lee breaks down what a unified telemetry datastore needs and why ClickHouse fits for logs and traces today, surveying ClickHouse-based platforms like Coroot, qryn, and SigNoz. |
| 2022-09 | [Log analytics using ClickHouse — Cloudflare (Monitorama 2022)](https://blog.cloudflare.com/log-analytics-using-clickhouse/) | Adapted transcript, slides, and video of Cloudflare's Monitorama 2022 talk on moving their high-volume request-error log analytics off Elasticsearch onto ClickHouse. |

---

## How-Tos & Guides

Evergreen docs first, then dated guides newest to oldest.

| Date | Link | Description |
|---|---|---|
| — | [Integrating OpenTelemetry for data collection (ClickHouse)](https://clickhouse.com/docs/observability/integrating-opentelemetry) | Official docs on receivers, processors, and exporters for a ClickHouse-powered observability solution. |
| — | [Observability engineering resources hub (ClickHouse)](https://clickhouse.com/resources/engineering/observability) | Reference on the SQL-based approach, using LogHouse's 16x compression multi-region setup as the model. |
| — | [OpenTelemetry ClickHouse Query Guide (Altinity)](https://altinity.com/useful-observability-queries/) | Practical SQL examples for querying OTel logs, metrics, and traces in ClickHouse, with common filters and Grafana visualization patterns. |
| — | [ClickHouse Monitoring Knowledge Base (Altinity)](https://kb.altinity.com/altinity-kb-setup-and-maintenance/altinity-kb-monitoring/) | Altinity KB reference on monitoring ClickHouse itself via system tables, ProfileEvents, metric_log, and Prometheus/Grafana. |
| 2026-03 | [How to Use ClickHouse with Open Source Observability Stacks (OneUptime)](https://oneuptime.com/blog/post/2026-03-31-clickhouse-open-source-observability/view) | Building a logs-metrics-traces platform with OpenTelemetry, Grafana, and Vector. |
| 2026-03 | [How to Use ClickHouse as a Backend for SigNoz (OneUptime)](https://oneuptime.com/blog/post/2026-03-31-clickhouse-signoz-backend/view) | Configuring and tuning SigNoz's ClickHouse storage for production, including sharded/replicated Helm deployment. |
| 2026-02 | [How to Configure the ClickHouse Exporter in the OTel Collector (OneUptime)](https://oneuptime.com/blog/post/2026-02-06-clickhouse-exporter-opentelemetry-collector/view) | Configuration patterns for schema optimization, compression, and materialized views on the exporter. |
| 2026-02 | [How to Use ClickHouse as a High-Performance OTel Backend (OneUptime)](https://oneuptime.com/blog/post/2026-02-06-clickhouse-high-performance-opentelemetry-backend/view) | Pairing ClickHouse with Grafana for RED-metric service dashboards built straight from OTel data. |
| 2026-01 | [How to Stream OpenTelemetry Data to ClickHouse (OneUptime)](https://oneuptime.com/blog/post/2026-01-21-clickhouse-opentelemetry/view) | Schema design for traces/metrics/logs plus Collector config, with sample Grafana panel SQL. |
| 2026-01 | [Cost-efficient open source observability playbook (ClickHouse)](https://clickhouse.com/resources/engineering/observability-cost-optimization-playbook) | Technical guide to ZSTD codecs, materialized views, and tiered storage for a 10x footprint reduction, plus a legacy-vendor migration path. |
| 2025-10 | [Production-Grade Observability with SigNoz, ClickHouse, OTel (Shivee Gupta)](https://medium.com/@ShiveeGupta/building-a-production-grade-observability-platform-with-signoz-clickhouse-and-opentelemetry-d7f09a5250f5) | Real-world architecture using Kafka as a buffer into a 3-shard/3-replica ClickHouse cluster with 90-day tiering. |
| 2024-09 | [An Introduction to the OpenTelemetry Collector (Altinity)](https://altinity.com/blog/an-introduction-to-the-opentelemetry-collector) | Primer on the Collector as a "universal translator" — receivers, processors, exporters, and connectors for routing metrics, traces, and logs. |
| 2024-09 | [Kubernetes Cluster Logging with ClickHouse and OpenTelemetry (Altinity)](https://altinity.com/blog/kubernetes-cluster-logging-with-clickhouse-and-opentelemetry) | Working demo wiring an OTel Collector daemonset with the filelog receiver to ship all Kubernetes cluster logs into ClickHouse, visualized in Grafana, with deployable Helm code. |

---

## Benchmarks

| Link | Description |
|---|---|
| [ClickHouse Benchmarks (Altinity)](https://altinity.com/benchmarks/) | Hub of Altinity and Altinity/Percona benchmark reports, including the parallel log processing and time-series performance studies most relevant to observability workloads. |
| [Benchmark blog archive (Altinity)](https://altinity.com/blog/tag/benchmark/) | Running collection of Altinity benchmark write-ups comparing ClickHouse against Redshift, TimescaleDB, and other engines. |
| [ClickBench (ClickHouse)](https://github.com/ClickHouse/ClickBench) | ClickHouse's public analytical database benchmark comparing query performance across dozens of engines on a real-world clickstream dataset. |


## Case Studies & User Stories

Ordered newest to oldest.

| Date | Link | Description |
|---|---|---|
| 2025-10 | [How Netflix optimized its petabyte-scale logging system (ClickHouse)](https://clickhouse.com/blog/netflix-petabyte-scale-logging) | Netflix's largest namespace ingests 5 PB/day; covers the log fingerprinting journey from ML to regex to ClickHouse. |
| 2025-06 | [Why OpenAI chose ClickHouse for petabyte-scale observability (ClickHouse)](https://clickhouse.com/blog/why-openai-uses-clickhouse-for-petabyte-scale-observability) | OpenAI ingests petabytes of logs daily, growing 20%+ monthly, across research, ChatGPT, and enterprise APIs. |
| 2025-03 | [ClickHouse Acquires HyperDX (ClickHouse)](https://www.businesswire.com/news/home/20250313954782/en) | Acquisition announcement noting the LogHouse transition off Datadog and ClickHouse's role behind eBay and Netflix observability.
