# Enterprise Linux Automation Lab

## Project Overview
A multi-server RHEL 9 infrastructure deployed on AWS and hardened to
enterprise security standards. This project demonstrates real-world
Linux sysadmin skills including cloud infrastructure design, OS
hardening, service deployment, and centralized logging — built
manually first, then fully automated with Ansible in Phase 2.

---

## Project Status

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Manual AWS build, OS hardening, service deployment, centralized logging | ✅ Complete |
| Phase 2 | Ansible fundamentals | 🔜 Up Next |
| Phase 3 | Automate Phase 1 with Ansible Roles and Vault | ⏳ Planned |
| Phase 4 | Monitoring with Prometheus + Grafana | ⏳ Planned |

---

## Architecture

**Cloud:** AWS (us-east-2)
**Network:** Custom VPC — 10.0.0.0/16 | Public Subnet — 10.0.1.0/24

| Node | Role | Private IP | Security Group | AMI |
|---|---|---|---|---|
| control01 | Log aggregator / future Ansible control node | 10.0.1.145 | sg-control | RHEL 9 |
| web01 | Apache web server | 10.0.1.220 | sg-web | RHEL 9 |
| db01 | MariaDB database | 10.0.1.224 | sg-db | RHEL 9 |

> Public IPs are dynamic and not documented here. SSH access is
> key-based only; credentials are never stored in this repository.

---

## Technologies Used

- **Cloud:** AWS EC2, VPC, IAM, Security Groups
- **OS:** Red Hat Enterprise Linux 9
- **Security:** SELinux (enforcing), firewalld, SSH hardening
- **Services:** Apache httpd, MariaDB, rsyslog (centralized logging)
- **Time Sync:** chrony
- **Coming in Phase 3:** Ansible, Ansible Vault
- **Coming in Phase 4:** Prometheus, Grafana

---

## Key Design Decisions

**Separate SSH key pairs per node**
Each instance uses its own key pair rather than a single shared key.
This mirrors enterprise environments where different teams manage
different servers — a compromised web server key does not grant
access to the database tier.

**Custom VPC over AWS default**
The default VPC uses permissive configurations unsuitable for a
hardened environment. A custom VPC provides explicit control over
subnetting, routing, and network boundaries from day one.

**Database not publicly accessible**
sg-db restricts port 3306 to traffic sourced from sg-web only.
The database tier is unreachable from the internet — traffic must
pass through the web tier first, enforcing network segmentation.

**ICMP restricted to VPC CIDR**
Ping is permitted between internal nodes (10.0.0.0/16) for
troubleshooting and health verification, but blocked from the
public internet.

**Centralized logging before automation**
rsyslog forwarding from web01 and db01 to control01 was established
manually in Phase 1. This mirrors the foundation of enterprise SIEM
tools like Splunk and ensures log correlation is in place before
the environment grows in Phase 3.

---

## Repository Structure

enterprise-linux-lab/
├── README.md
├── docs/
│   ├── architecture.md       # Network design and decisions
│   ├── runbook.md            # Step-by-step build documentation
│   └── lessons-learned.md   # Troubleshooting log and takeaways
└── screenshots/              # Console and terminal verification shots


---

## Documentation

All build steps, design decisions, and verification outputs are
documented in [`docs/runbook.md`](docs/runbook.md). Real-world
troubleshooting notes are tracked in
[`docs/lessons-learned.md`](docs/lessons-learned.md).
