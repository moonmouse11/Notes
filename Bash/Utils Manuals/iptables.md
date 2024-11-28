# iptables
***
## Basic
**iptables/ip6tables** — administration tool for IPv4/IPv6 packet filtering and NAT
```bash
       iptables [-t table] {-A|-C|-D|-V} chain rule-specification

       ip6tables [-t table] {-A|-C|-D|-V} chain rule-specification

       iptables [-t table] -I chain [rulenum] rule-specification

       iptables [-t table] -R chain rulenum rule-specification

       iptables [-t table] -D chain rulenum

       iptables [-t table] -S [chain [rulenum]]

       iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]

       iptables [-t table] -N chain

       iptables [-t table] -X [chain]

       iptables [-t table] -P chain target

       iptables [-t table] -E old-chain-name new-chain-name

       rule-specification = [matches...] [target]

       match = -m matchname [per-match-options]

       target = -j targetname [per-target-options]

```
***
## Description
`Iptables`  and  `ip6tables`  are used to set up, maintain, and inspect the tables of IPv4 and IPv6 packet filter rules in the Linux kernel.  Several different tables may be defined.  Each table contains a number of built-in chains and may also contain user-defined chains.
Each chain is a list of rules which can match a set of packets.  Each rule specifies what to do with a packet that matches.  This is called a `target`, which may be a jump to a user-defined chain in the same table.
***
## Example
```bash
iptables -L     # Выводит список всех подключений системы
iptables -F     # Удаляет все правила подключений
```
***
