## June 16, 2026 — ICMP Not Enabled for Internal Network Verification

**What I was doing:** Verifying communication between instances by 
pinging from web01 to other nodes after SSH was confirmed working.

**What happened:** 100% packet loss on all ping attempts.
```bash
[root@web01 ec2-user]# ping -c3 control01
PING control01 (10.0.1.145) 56(84) bytes of data.
--- control01 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2034ms
```

**Root cause:** ICMP traffic was not permitted in the AWS security 
groups. SSH (port 22) was open but ICMP is a separate protocol that 
requires its own rule.

**How I fixed it:** Added an inbound ICMP rule to all three security 
groups scoped to `10.0.0.0/16` — allowing ping within the VPC only, 
not from the public internet.

**What I learned:** SSH confirms a port is open but doesn't verify 
general network reachability between nodes. ICMP is a lightweight 
tool for quick connectivity checks without opening additional service 
ports. Scoping it to the VPC CIDR keeps it internal — a good balance 
between usability and security.
