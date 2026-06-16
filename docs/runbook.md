## AWS Account Hardening

### Why This Matters
Root account compromise is one of the most common and catastrophic 
cloud security incidents. Locking down access before deploying any 
infrastructure is standard practice in enterprise environments.

### Steps Completed
- Enabled MFA on root account (authenticator app)
- Created IAM user `lab-admin` — all work performed under this identity
- Root account credentials stored securely and not used operationally
- Password policy enforced at account level

### Verification
Logged in as lab-admin successfully. Root account MFA confirmed active.


## Instance Inventory

| Hostname | Role | Private IP | Public IP | Security Group | AMI |
|---|---|---|---|---|---|
| control01 | Ansible control / admin | 10.0.1.145 | 13.58.233.127 | sg-control | RHEL 9 |
| web01 | Apache web server | 10.0.1.220 | 3.138.170.33 | sg-web | RHEL 9 |
| db01 | MariaDB database | 10.0.1.224 | 3.142.199.39 | sg-db | RHEL 9 |

*Public IPs are dynamic and reassigned on instance stop/start.*

## Hostnames
Set hostnames on all instances to match inventory names for clarity 
during SSH sessions and future Ansible inventory configuration.

## SSH Access

```bash
ssh -i control01.pem ec2-user@13.58.233.127
ssh -i db01.pem ec2-user@3.142.199.39
ssh -i web01.pem ec2-user@3.138.170.33
```

**Note:** Seperate Key files for each instance for compliance purposes (i.e. Different teams manage different servers). Key files must have permissions 400. Store Key files securely — never commit it to the repository.

### ICMP Rule Addition

Although not included in the initial configuration, all security groups were updated to allow ICMP (ping) traffic sourced from within the VPC (10.0.0.0/16). This enables internal network verification between instances without exposing ping to the public internet.


