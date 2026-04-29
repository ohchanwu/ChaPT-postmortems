## First SSH login in the crash boot

[ec2-user@ip-10-0-0-20 postmortem]$ grep "Accepted publickey" ssh-events.log | head -1
Feb 19 20:38:34 ip-10-0-0-10.ec2.internal sshd[196675]: Accepted publickey for ec2-user from 203.0.113.5 port 58541 ssh2: ED25519 SHA256:[REDACTED]

## First DBus/timeout error (the cascade begins here)

[ec2-user@ip-10-0-0-20 postmortem]$ grep -i "timed out" final-hour.log | head -1
Apr 17 07:44:23 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Failed to abandon session scope, ignoring: Connection timed out

## First Docker health check timeout

[ec2-user@ip-10-0-0-20 postmortem]$ grep -i "health check" docker-events.log | head -1
Feb 19 20:45:23 ip-10-0-0-10.ec2.internal dockerd[1580]: time="2026-02-19T20:44:55.435580702Z" level=warning msg="Health check for container 3c870bdfa82b06246371e1b56d471940cba9dd7efff7c43b552b86669e5d796d error: timed out starting health check for container 3c870bdfa82b06246371e1b56d471940cba9dd7efff7c43b552b86669e5d796d"

## Power button / shutdown signal

[ec2-user@ip-10-0-0-20 postmortem]$ grep -i "Power key" shutdown-events.log | head -1
Apr 17 07:55:31 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Power key pressed short.

## Last log entry before death

[ec2-user@ip-10-0-0-20 postmortem]$ tail -5 crash-boot-tail.log
Apr 17 07:55:31 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Power key pressed short.
Apr 17 07:55:31 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Powering off...
Apr 17 07:55:46 ip-10-0-0-10.ec2.internal amazon-ssm-agent[1571]: request expired, resigning
Apr 17 07:55:57 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Failed to get load state of poweroff.target: Connection timed out
Apr 17 07:55:57 ip-10-0-0-10.ec2.internal systemd-logind[1407]: Failed to execute poweroff operation: Connection timed out
