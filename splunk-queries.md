# Splunk Detection Queries

## SSH Brute Force
```spl
index=* host="ubuntu-victim" "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| where count > 5
```

## Port Scan Detection
```spl
index=* host="ubuntu-victim" "UFW BLOCK"
| rex "SRC=(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex "DPT=(?<dest_port>\d+)"
| stats dc(dest_port) as unique_ports_hit, count as total_attempts by src_ip
| where unique_ports_hit > 5
```

## Privilege Escalation Attempt
```spl
index=* host="ubuntu-victim" ("pam_unix(sudo:auth): authentication failure" OR "pam_unix(su:auth): authentication failure")
| rex "logname=(?<user>\S+)"
| rex "user=(?<target_user>\S+)"
| eval escalation_type=if(match(_raw, "sudo:auth"), "sudo", "su")
| stats count as failed_attempts, values(escalation_type) as methods_used by user, target_user
| where failed_attempts > 2
```
