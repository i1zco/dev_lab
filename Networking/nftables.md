# nftables Learning Notes

This document summarizes my hands-on practice while learning **nftables** on Ubuntu.

---

# Create an Input Chain

## Goal

Create a filter table and an input chain with a default **Drop** policy.

## Commands

```bash
sudo nft add table inet filter

sudo nft add chain inet filter input "{ type filter hook input priority 0; policy drop; }"
```

## Verify

```bash
sudo nft list ruleset
```

## Expected Output

```text
table inet filter {
    chain input {
        type filter hook input priority filter;
        policy drop;
    }
}
```

## Explanation

- `table inet filter` creates a table for both IPv4 and IPv6.
- `chain input` processes packets destined for the local machine.
- `hook input` attaches the chain to incoming traffic.
- `priority 0` defines when this chain is evaluated.
- `policy drop` blocks any packet that does not match an allow rule.

## Observation

The input chain was successfully created with the default policy set to **Drop**.

## What I Learned

- How to create a table and an input chain.
- The purpose of hooks and priorities.
- Why a default **Drop** policy provides better security.

> **Note**
>
> Commands executed with `nft add` modify the running firewall only. They do **not** automatically update `/etc/nftables.conf`.

---

# Allow Essential Services

## Goal

Allow SSH, HTTP and HTTPS traffic.

## Commands

```bash
sudo nft add rule inet filter input tcp dport 22 accept

sudo nft add rule inet filter input tcp dport 80 accept

sudo nft add rule inet filter input tcp dport 443 accept
```

## Verify

```bash
sudo nft list ruleset
```

## Expected Output

```text
tcp dport 22 accept
tcp dport 80 accept
tcp dport 443 accept
```

## Explanation

- Port **22** → SSH
- Port **80** → HTTP
- Port **443** → HTTPS

Packets matching these destination ports are accepted.

## Result

- SSH allowed.
- HTTP allowed.
- HTTPS allowed.
- All other ports remain blocked by the default policy.

## What I Learned

- How to allow services using destination ports.
- Why the default drop policy improves security.
- Why rule order is important.

> **Security Note**
>
> In production environments, SSH should normally be restricted to trusted IP addresses instead of allowing access from everywhere.

---

# Matching Packet Fields

## Goal

Understand how nftables matches packets before applying a rule.

## Packet Fields Learned

- Source IP (`saddr`)
- Destination IP (`daddr`)
- Protocol (`TCP` / `UDP`)
- Destination Port (`dport`)
- Input Interface (`iif`)

## Example Rule

```nft
ip saddr 192.168.1.50 tcp dport 22 accept
```

## Explanation

- `ip` → IPv4 packet
- `saddr` → Source IP address
- `tcp` → TCP protocol
- `dport 22` → Destination port 22 (SSH)
- `accept` → Allow the packet

The packet must satisfy **all conditions** before the action is applied.

## What I Learned

- nftables evaluates multiple packet fields.
- A rule can combine several matching conditions.
- Every condition in the rule must match before the action is executed.

---

# Using the include Directive

## Goal

Organize firewall rules into multiple configuration files.

## Example Directory Structure

```text
/etc/nftables/
├── nftables.conf
└── input-rules.d/
    ├── ssh.conf
    ├── web.conf
    └── dns.conf
```

## Example Main Configuration

```nft
table inet filter {

    chain input {

        type filter hook input priority filter;

        policy drop;

        include "/etc/nftables/input-rules.d/*.conf"
    }
}
```

## Example Rule Files

### ssh.conf

```nft
tcp dport 22 accept
```

### web.conf

```nft
tcp dport 80 accept
tcp dport 443 accept
```

### dns.conf

```nft
udp dport 53 accept
tcp dport 53 accept
```

## Explanation

The `include` directive loads every `.conf` file inside the directory.

This keeps the main configuration file clean while separating rules by service.

## Benefits

- Easier maintenance.
- Cleaner configuration.
- Better organization.
- Easier collaboration between administrators.

## What I Learned

- How to split firewall rules into multiple files.
- Why modular configurations are easier to maintain.

---

# Symbolic Variables

## Goal

Reduce repetition by using reusable variables.

## Variables

```nft
define IF_LAN = eth0
define VPN_NET = 10.8.0.0/24
```

## Example

```nft
iif $IF_LAN ip saddr $VPN_NET tcp dport 22 accept
```

## Explanation

- `$IF_LAN` represents the LAN interface.
- `$VPN_NET` represents the VPN subnet.
- Variables make configuration easier to update.

If the interface name or VPN subnet changes, only the variable needs to be updated.

## Benefits

- Easier maintenance.
- Avoid repeated values.
- Cleaner configuration.
- Faster updates.

## What I Learned

- `define` creates reusable symbolic variables.
- Variables improve readability and maintainability.
- One modification can update multiple firewall rules.

---

# Summary

During these labs, I learned how to:

- Create nftables tables and chains.
- Configure a secure default drop policy.
- Allow selected network services.
- Match packets using multiple fields.
- Organize rules with the `include` directive.
- Simplify configurations using symbolic variables.

These exercises helped me understand the fundamentals of **nftables** and how firewall rules are structured and managed on Ubuntu.
