# ChaPT post-mortems

_Read this in: [한국어](README.ko.md)_

Post-incident reviews and hardening artifacts from the operation of [Cha Physical Therapy's](https://chaphysicaltherapy.com) booking and analytics platform — a small Go + MySQL web application I built and operate solo.

Written as portfolio artifacts: blameless, evidence-driven, and including the configs that closed the loop on each incident's action items.

## Incidents

- **[2026-04-17 — gp3 throughput saturation cascade](2026-04-17/2026-04-17.md)** ([한국어](2026-04-17/2026-04-17.ko.md)) — A set of unoptimized analytics queries saturated the EC2 instance's gp3 root volume, cascading into a ~3 hour outage via systemd DBus timeouts, Docker health-check failures, and stuck SSH. Recovery via fresh instance + Elastic IP reassignment; no data loss.

## Layout

```
.
└── 2026-04-17/                         # Per-incident folder
    ├── 2026-04-17.md                   # The post-mortem (English)
    ├── 2026-04-17.ko.md                # The post-mortem (Korean)
    ├── buffer.md                       # Raw log evidence cited in the timeline
    ├── CloudWatchGraphs/               # Inline-referenced CloudWatch screenshots
    └── hardening/                      # Action items implemented in response to this incident
        ├── my.cnf                      # MySQL tuning (action items #2, #4)
        └── docker-compose-logging.yml  # Docker log rotation (action item #6)
```

## About ChaPT

Single-developer project — I designed, built, deployed, and operate it. Go backend, MySQL, Stripe Elements for payments, hosted on a single EC2 instance behind an Elastic IP. The post-mortem here documents one of the things that goes wrong when a one-person stack runs into infrastructure ceilings nobody warned you about.

## A note on sanitization

The author's SSH source IP and the SSH host fingerprint in `buffer.md` have been replaced with placeholders (`203.0.113.5`, `[REDACTED]`). AWS internal hostnames have been generalized to RFC 1918 placeholders (`ip-10-0-0-10`, `ip-10-0-0-20`). The forensic narrative is otherwise unchanged.
