# Level 2 — Secure Overlay (WireGuard)

## Goal

Deploy encrypted communication between lab hosts using WireGuard VPN.

The goal is to build a secure overlay network on top of the existing infrastructure created in Level 1.

## Components

* WireGuard VPN
* encrypted communication between hosts
* secure tunnel configuration

## Key Concepts

WireGuard creates a lightweight encrypted tunnel between nodes.

This allows secure communication even if the underlying network is not trusted.

## Verification

Check WireGuard interface:

```id="w6t3ze"
ip a
```

Check tunnel status:

```id="mxy7rk"
sudo wg show
```

Verify encrypted connectivity between nodes.
