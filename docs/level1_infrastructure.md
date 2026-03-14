# Level 1 — Infrastructure Base

## Goal

Build a basic Linux infrastructure with static networking, SSH access and persistent logging.

## Components

* static IP configuration
* SSH remote administration
* persistent system logs

## Verification

Test network connectivity:

```
ping 10.0.0.1
ping 10.0.0.10
ping 10.0.0.21
ping 10.0.0.22
```

Verify SSH access:

```
ssh user@10.0.0.10
```

Check persistent logs:

```
journalctl -b -1
```
