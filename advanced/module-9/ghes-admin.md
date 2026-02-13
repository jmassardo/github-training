---
layout: training-module
title: "GHES Administration"
permalink: /advanced/module-9/ghes-admin/
module_number: 9
module_title: "Enterprise Administration"
section_number: 9
total_sections: 10
phase: advanced
estimated_time: "35 min"
module_index: /advanced/module-9/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-9/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/advanced/module-9/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-9/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/advanced/module-9/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/advanced/module-9/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-9/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-9/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
  - title: "GHES vs GHEC"
    url: "/advanced/module-9/ghes-vs-ghec/"
    short_title: "GHES vs GHEC"
    icon: "⚖️"
  - title: "GHES Administration"
    url: "/advanced/module-9/ghes-admin/"
    short_title: "GHES Admin"
    icon: "🖥️"
  - title: "Migrating to GHEC"
    url: "/advanced/module-9/migrations/"
    short_title: "Migrations"
    icon: "🔄"
toc: true
prev_section:
  url: /advanced/module-9/ghes-vs-ghec/
  title: "GHES vs GHEC"
next_section:
  url: /advanced/module-9/migrations/
  title: "Migrating to GHEC"
---

# GitHub Enterprise Server Administration

## Why CSMs Need to Understand GHES Administration

Many enterprise customers run GitHub Enterprise Server in their own data centers or private cloud. As a CSM, you won't be the one racking servers — but you need to understand GHES architecture, maintenance workflows, and operational concerns so you can:

- **Set expectations** around upgrade timelines and maintenance windows
- **Troubleshoot** customer issues or escalate effectively to support
- **Advise** on high availability and disaster recovery configurations
- **Guide** capacity planning discussions as their usage grows

> 💡 **Think of it like this:** You don't need to be a mechanic, but if you're selling cars, you need to understand how the engine works, when oil changes are needed, and what the warranty covers.

---

## GHES Architecture Overview

### Core Components

A GitHub Enterprise Server instance is a self-contained appliance that runs as a virtual machine:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph GHES["GHES Virtual Appliance"]
        direction TB
        subgraph Services["Frontend Services"]
            WebUI["Web UI\n(HTTPS)"]
            GitSSH["Git SSH\n(Port 22)"]
            API["API Server\n(REST/GraphQL)"]
        end
        subgraph Data["Data Services"]
            MySQL["MySQL\n(data)"]
            Redis["Redis\n(cache)"]
            ES["Elasticsearch\n(search)"]
        end
        subgraph Infrastructure["Infrastructure Services"]
            MinIO["MinIO\n(blobs)"]
            Memcached["Memcached"]
            Actions["GitHub Actions\n(optional)"]
        end
        Storage["Git Repository Storage\n(NFS or attached storage)"]
    end
</div>
</div>

### Supported Platforms

| Platform | Supported Formats | Notes |
|----------|------------------|-------|
| **AWS** | AMI (EC2) | Most common cloud deployment |
| **Azure** | VHD | Native Azure integration |
| **GCP** | GCE image | Google Cloud Compute Engine |
| **VMware** | OVA | On-premises VMware vSphere |
| **Hyper-V** | VHD | Microsoft Hyper-V |
| **OpenStack KVM** | QCOW2 | OpenStack deployments |

### System Requirements

| Component | Minimum | Recommended (up to 5,000 users) | Large (5,000-25,000 users) |
|-----------|---------|--------------------------------|---------------------------|
| **vCPUs** | 4 | 8 | 16-32 |
| **RAM** | 32 GB | 64 GB | 96-128 GB |
| **Root Storage** | 200 GB SSD | 200 GB SSD | 200 GB SSD |
| **Data Storage** | 100 GB SSD | 250+ GB SSD | 500+ GB SSD |

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>

Storage grows with repository count and size. When advising customers, ask about their largest repositories, whether they use Git LFS, and how much CI/CD artifact storage they expect. These factors heavily influence storage requirements.
</div>

---

## Installation & Initial Setup

### Installation Workflow

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  A["1. Download GHES Image<br/>(from enterprise.github.com)"] --> B["2. Deploy VM on Platform<br/>(AWS, Azure, VMware, etc.)"]
  B --> C["3. Configure DNS<br/>(github.yourcompany.com)"]
  C --> D["4. Access Management Console<br/>(https://hostname:8443)"]
  D --> E["5. Upload License File<br/>(.ghl file from GitHub)"]
  E --> F["6. Configure Settings<br/>(Auth, email, storage, etc.)"]
  F --> G["7. Create Site Admin Account"]
  G --> H["8. Instance Ready!"]
</div>
</div>

### Management Console

The **Management Console** (port 8443) is the primary interface for GHES server administration. It's separate from the regular GitHub web interface and provides:

- **Settings** — Authentication, email, GitHub Connect, GitHub Actions, Pages, Packages
- **Maintenance** — Backup status, upgrade management, cluster health
- **Monitoring** — System health, resource utilization, service status
- **Support Bundle** — Generate diagnostic bundles for GitHub Support

<div class="callout callout-warning" markdown="1">
<div class="callout-title">⚠️ Important Distinction</div>

Don't confuse the **Management Console** (infrastructure admin, port 8443) with the **Site Admin Dashboard** (GitHub admin, accessible via the regular web UI). The Management Console manages the server itself; the Site Admin Dashboard manages users, organizations, and GitHub-level settings.
</div>

### `ghe-` Command Line Utilities

GHES includes a suite of administrative command-line tools accessible via SSH:

| Command | Purpose |
|---------|---------|
| `ghe-config` | View and modify instance configuration |
| `ghe-config-apply` | Apply configuration changes |
| `ghe-maintenance` | Enable/disable maintenance mode |
| `ghe-upgrade` | Upgrade GHES to a new version |
| `ghe-backup-utils` | Backup and restore operations |
| `ghe-support-bundle` | Generate support bundles |
| `ghe-repl-status` | Check replication status (HA) |
| `ghe-repl-promote` | Promote replica to primary (failover) |
| `ghe-actions-check` | Verify GitHub Actions configuration |
| `ghe-ssl-certificate-setup` | Configure SSL/TLS certificates |

---

## Upgrades & Maintenance

### GHES Release Cycle

GitHub Enterprise Server follows a regular release cadence:

- **Feature releases** (e.g., 3.12, 3.13) — Quarterly, include new features
- **Patch releases** (e.g., 3.12.1, 3.12.2) — As needed, include bug fixes and security patches
- **Support window** — Each feature release is supported for approximately 12 months

### Upgrade Process

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  A["1. Review Release Notes<br/>& Breaking Changes"] --> B["2. Take Full Backup<br/>(ghe-backup-utils)"]
  B --> C["3. Enable Maintenance Mode<br/>(ghe-maintenance -s)"]
  C --> D["4. Download Upgrade Package<br/>(.pkg file)"]
  D --> E["5. Upload & Install Package<br/>(Management Console or CLI)"]
  E --> F["6. Wait for Services to Restart<br/>(15-45 minutes typical)"]
  F --> G["7. Validate Instance<br/>(smoke tests)"]
  G --> H["8. Disable Maintenance Mode<br/>(ghe-maintenance -u)"]
  H --> I["9. Verify & Notify Users"]
</div>
</div>

### Upgrade Best Practices

| Practice | Description |
|----------|-------------|
| **Test first** | Upgrade a staging/test instance before production |
| **Read release notes** | Check for deprecations, breaking changes, and migration notes |
| **Schedule maintenance windows** | Communicate downtime to users in advance |
| **Back up before upgrading** | Always run a full backup with `ghe-backup-utils`) |
| **Don't skip versions** | Sequential upgrades are required (3.11→3.12→3.13, not 3.11→3.13) |
| **Keep within support window** | Unsupported versions don't receive security patches |

<div class="callout callout-info" markdown="1">
<div class="callout-title">📋 Upgrade Support</div>

Customers can use **hotpatching** for patch releases on supported versions, which applies updates without a full maintenance window. Feature release upgrades still require maintenance mode and a restart.
</div>

---

## High Availability (HA)

### What HA Provides

High Availability configures a **passive replica** of the primary GHES instance that can take over if the primary fails:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph Primary["Primary GHES (Active)"]
        P1["✅ Serves traffic"]
        P2["✅ Writes accepted"]
    end
    subgraph Replica["Replica GHES (Passive)"]
        R1["⏸️ Standby only"]
        R2["⏸️ Read-only"]
    end
    Primary -- "Async Replication" --> Replica
    Primary --> PS["Primary Storage\n(all data)"]
    Replica --> RS["Replica Storage\n(replicated copy)"]
</div>
</div>

### HA Key Concepts

| Concept | Description |
|---------|-------------|
| **Replication** | Asynchronous — data is replicated from primary to replica with a small delay |
| **Failover** | Manual — an administrator runs `ghe-repl-promote` on the replica |
| **DNS Update** | After failover, DNS must be updated to point to the new primary |
| **RPO** | Near-zero (seconds of data loss depending on replication lag) |
| **RTO** | Minutes (depends on failover procedure and DNS propagation) |

### Setting Up HA

1. Provision a second VM with the same specifications as the primary
2. Configure the replica: `ghe-repl-setup PRIMARY_IP`
3. Start replication: `ghe-repl-start`
4. Monitor status: `ghe-repl-status`

### When to Recommend HA

- Customer has **SLA requirements** for GitHub availability
- Development teams span **multiple time zones** and need continuous access
- GitHub is a **critical path** dependency for deployment pipelines
- Customer has compliance requirements around **business continuity**

---

## Clustering

### What Clustering Provides

For very large deployments (5,000+ users), **clustering** distributes GHES services across multiple nodes for horizontal scaling:

| Component | Standalone | HA | Clustering |
|-----------|-----------|-----|------------|
| **Nodes** | 1 | 2 (primary + replica) | 3+ (distributed services) |
| **Scaling** | Vertical only | Vertical only | Horizontal |
| **Failover** | None | Manual | Automatic (per-service) |
| **User Range** | Up to ~5,000 | Up to ~5,000 | 5,000-25,000+ |
| **Complexity** | Low | Medium | High |
| **Use Case** | Small-medium orgs | Business continuity | Large enterprises |

### Cluster Architecture

In a cluster, individual GHES services (Git, web, search, storage) are distributed across nodes:

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
    LB["Load Balancer"] --> N1 & N2 & N3
    subgraph N1["Node 1"]
        N1Web["Web"]
        N1API["API"]
        N1Jobs["Jobs"]
    end
    subgraph N2["Node 2"]
        N2Web["Web"]
        N2Search["Search"]
        N2Storage["Storage"]
    end
    subgraph N3["Node 3"]
        N3Git["Git"]
        N3Pages["Pages"]
        N3Storage["Storage"]
    end
</div>
</div>

<div class="callout callout-warning" markdown="1">
<div class="callout-title">⚠️ Clustering Complexity</div>

Clustering is significantly more complex to set up and operate than standalone or HA configurations. Only recommend it for customers who genuinely need horizontal scaling. Most enterprises with fewer than 5,000 users are better served by a standalone instance with HA replication.
</div>

---

## Backup & Disaster Recovery

### GitHub Enterprise Server Backup Utilities

`ghe-backup-utils` is the official backup tool for GHES. It performs incremental, snapshot-based backups:

### What Gets Backed Up

| Data | Included | Notes |
|------|----------|-------|
| Git repositories | ✅ | Full repository data including all branches and tags |
| MySQL database | ✅ | Users, organizations, issues, PRs, comments |
| Redis data | ✅ | Background job queues |
| Elasticsearch indices | ✅ | Search indexes (can be rebuilt) |
| Configuration | ✅ | Instance settings and certificates |
| Audit logs | ✅ | Enterprise audit trail |
| GitHub Actions data | ✅ | Workflow runs and logs |
| GitHub Packages data | ✅ | Published packages |

### Backup Best Practices

| Practice | Recommendation |
|----------|----------------|
| **Frequency** | At least daily; hourly for critical deployments |
| **Storage** | Separate host from GHES instance (different failure domain) |
| **Retention** | Keep at least 7 days; 30+ days for compliance |
| **Testing** | Regularly test restore procedures (quarterly at minimum) |
| **Offsite copies** | Replicate backups to a secondary location |
| **Monitoring** | Alert on backup failures immediately |

### Disaster Recovery Strategies

| Strategy | RPO | RTO | Complexity | Cost |
|----------|-----|-----|------------|------|
| **Backup-only** | Hours (last backup) | Hours (restore from backup) | Low | Low |
| **HA Replication** | Seconds | Minutes (manual failover) | Medium | Medium |
| **Geo-Replication** | Seconds | Minutes (DNS failover) | High | High |
| **HA + Backup** | Seconds (HA) / Hours (backup as last resort) | Minutes | Medium | Medium |

---

## GitHub Actions on GHES

### Key Differences from GHEC

| Aspect | GHEC | GHES |
|--------|------|------|
| **GitHub-hosted runners** | ✅ Available | ❌ Not available |
| **Self-hosted runners** | ✅ Available | ✅ Required |
| **Actions from GitHub.com** | ✅ Direct access | ⚠️ Requires GitHub Connect or manual sync |
| **Included minutes** | ✅ Included in plan | N/A (self-hosted) |
| **External storage** | Not needed | ✅ Required (S3, Azure Blob, or MinIO) |

### Configuring Actions on GHES

Actions on GHES requires:

1. **External blob storage** — Amazon S3, Azure Blob Storage, or MinIO (on-premises S3-compatible)
2. **Self-hosted runners** — Customer-managed compute for running workflows
3. **GitHub Connect** (optional) — For syncing actions from GitHub.com Marketplace

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>

When customers ask about Actions on GHES, emphasize that they need to plan for **runner infrastructure**. This includes capacity planning for concurrent jobs, runner groups for security isolation, and runner images for different workloads. The shift from GitHub-hosted to self-hosted runners is often the biggest adjustment for teams migrating between platforms.
</div>

---

## Monitoring & Troubleshooting

### Built-in Monitoring

GHES exposes monitoring data through several channels:

| Method | What It Provides | How to Access |
|--------|-----------------|---------------|
| **Management Console** | System health dashboard | `https://hostname:8443` |
| **`ghe-*` CLI tools** | Service status and diagnostics | SSH to instance |
| **SNMP** | Standard monitoring protocol | Configure in Management Console |
| **Collectd** | Detailed system metrics | Forward to Graphite/InfluxDB |
| **Syslog** | System and application logs | Forward to external log aggregator |

### Common Issues & Resolutions

| Issue | Symptoms | Resolution |
|-------|----------|------------|
| **High memory usage** | Slow response times, OOM errors | Increase VM RAM, check for runaway processes |
| **Storage full** | Push failures, Actions failures | Add storage, clean up old data, enable Git GC |
| **Replication lag** | HA replica falls behind | Check network bandwidth, storage I/O |
| **Search issues** | Missing search results | Rebuild Elasticsearch index (`ghe-es-search-repair`) |
| **SSL certificate expiry** | Browser warnings, API failures | Renew certificate via Management Console |
| **Slow Git operations** | Clone/push timeouts | Check repository size, enable Git maintenance |

### Support Bundles

When escalating to GitHub Support, generate a support bundle:

```bash
# Generate a support bundle (includes logs, diagnostics, and configuration)
ssh -p 122 admin@HOSTNAME -- 'ghe-support-bundle -o' > support-bundle.tgz

# Generate a targeted support bundle for a specific time range
ssh -p 122 admin@HOSTNAME -- 'ghe-support-bundle -o -t 2026-02-01' > support-bundle.tgz
```

---

## CSM Conversation Guide: GHES Operations

### Discovery Questions for GHES Customers

- "Who manages your GHES instance today? Is it a dedicated team or shared responsibility?"
- "How often are you running upgrades? Are you on the latest supported version?"
- "Do you have a high availability configuration? What's your current RPO/RTO?"
- "How are your backups configured? When was the last restore test?"
- "Are you using GitHub Actions on GHES? What does your runner infrastructure look like?"
- "Have you considered GitHub Connect for access to the Actions marketplace and Dependabot alerts?"

### Red Flags to Watch For

| Red Flag | Risk | Your Action |
|----------|------|------------|
| Running an unsupported GHES version | No security patches | Urgently recommend upgrade path |
| No HA configured | Single point of failure | Discuss business continuity requirements |
| No backup testing | Untested disaster recovery | Recommend quarterly restore drills |
| Maxed-out storage | Service interruption risk | Plan storage expansion or cleanup |
| No monitoring | Blind to performance issues | Recommend SNMP/collectd integration |

---

## Additional Resources

- 📖 [Installing GitHub Enterprise Server](https://docs.github.com/en/enterprise-server/admin/installation)
- 📖 [Configuring high availability](https://docs.github.com/en/enterprise-server/admin/enterprise-management/configuring-high-availability)
- 📖 [Configuring clustering](https://docs.github.com/en/enterprise-server/admin/enterprise-management/configuring-clustering)
- 📖 [Configuring backups](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-your-enterprise/configuring-backups-on-your-appliance)
- 📖 [GHES release notes](https://docs.github.com/en/enterprise-server/admin/release-notes)
- 📖 [Management Console](https://docs.github.com/en/enterprise-server/admin/configuration/administering-your-instance-from-the-management-console)
- 📖 [Command-line utilities](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-your-enterprise/command-line-utilities)
- 📖 [Monitoring your instance](https://docs.github.com/en/enterprise-server/admin/enterprise-management/monitoring-your-appliance)
