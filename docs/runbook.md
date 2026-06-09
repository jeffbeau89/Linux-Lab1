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
