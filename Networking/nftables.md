

# Create an Input Chain

## Command

```bash
sudo nft add table inet filter
sudo nft add chain inet filter input "{ type filter hook input priority 0; policy drop; }"
```

## Verify

```bash
sudo nft list ruleset
```

## Observation

The Input chain was created with the default policy set to Drop.

## Lesson Learned

The default policy blocks all packets that do not match a rule.






# Allow Essential Services

## Goal

Allow SSH, HTTP and HTTPS traffic.

## Commands

```bash
sudo nft add rule inet filter input tcp dport 22 accept
sudo nft add rule inet filter input tcp dport 80 accept
sudo nft add rule inet filter input tcp dport 443 accept
```

## Verification

```bash
sudo nft list ruleset
```

## Result

- SSH allowed.
- HTTP allowed.
- HTTPS allowed.
- Other ports remain blocked by the default policy.

## What I learned

- How to allow services using destination ports.
- Why the default drop policy improves security.
- Why rule order is important.





# Matching Packet Fields

## Goal

Understand how nftables matches packets.

## Packet fields learned

- Source IP (saddr)
- Destination IP (daddr)
- Protocol (TCP / UDP)
- Destination Port (dport)
- Interface (iif)

## Example Rule

```text
ip saddr 192.168.1.50 tcp dport 22 accept
```

## What I learned

- nftables evaluates multiple packet fields before making a decision.
- A rule can combine several conditions.
- Packets must match all conditions in the rule before the action is applied.




# Using the include Directive

## Goal

Learn how to organize nftables configuration into multiple files.

## Example Structure

```text
/etc/nftables/
├── nftables.conf
└── input-rules.d/
    ├── ssh.conf
    ├── web.conf
    └── dns.conf
```

## What I learned

- The `include` directive allows loading rules from multiple files.
- Organizing rules by service makes maintenance easier.
- The main configuration file becomes cleaner and easier to read.




# Symbolic Variables

## Goal

Learn how to simplify nftables configuration using symbolic variables.

## Variables

```nft
define IF_LAN = eth0
define VPN_NET = 10.8.0.0/24
```

## Benefits

- Easier maintenance.
- Avoid repeating values.
- One change updates multiple rules.

## What I learned

`define` creates reusable variables that make firewall configurations cleaner and easier to maintain.
