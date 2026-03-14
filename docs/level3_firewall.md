# Level 3 — Firewall Control (iptables)

## Goal

Implement strict firewall control using the deny-by-default model.

The objective of this level is to enforce network segmentation and limit communication between nodes unless explicitly allowed.

## Architecture Idea

The gateway node **GW-LAB** acts as the central firewall for the lab network.

Traffic between hosts is inspected and controlled using iptables rules.

The firewall policy follows the **deny-by-default** approach, meaning that all traffic is blocked unless a rule explicitly allows it.

## Key Concepts

* iptables firewall rules
* deny-by-default policy
* traffic filtering
* kernel logging

## Key Commands

Example commands used to inspect firewall rules:

```
sudo iptables -L -n -v
```

Check rule counters and packet statistics.

## Verification

Verify that the firewall policy is active:

```
sudo iptables -L -n -v
```

Confirm that only allowed services remain reachable.

Test connectivity between hosts to confirm that unauthorized traffic is blocked.
