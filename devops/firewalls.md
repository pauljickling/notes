# Firewalls

There are many ways to configure firewalls, most cloud providers have their own interfaces for creating firewall rules. These are desirable because it is a generic interface that can be applied across a range of cloud resources in a consistent way. However this is simply an examination of how to create a firewall through the Linux iptables interface.

For a Debian system, the iptables rules are written to `/etc/iptables/rules.v4`. You can use the `iptables` CLI to declare rules, or you can edit the file directly. For machines that accept ipv6 traffic there is also the corresponding `/etc/iptables/rules.v6` file and `ip6tables` CLI tool.

The `rules.v4` file begins with the line specifying the type of table to use. For example, `*filter` declares that the subsequent lines are how to filter incoming and outgoing traffic. `filter` is the default table. Other types of tables include `nat`, `mangle`, `raw`, and `security`.

In terms of a data structure, firewall rules can be thought of as a list where rules can be included either at the head or the tail of the list. The documentation for iptables refers to this type of list as a "chain", and in fact there are several specific chains you use (more on that in a moment). When using the `iptables` CLI before writing a rule you will use the flags `-A` for appending a rule to the chain, `-I` for inserting, `-D` for deleting, and `-C` for checking if a rule exists in the chain (useful for debugging whether a rule is included or not, which isn't very obvious if the actual config file is written in a confusing way).

When thinking about the order of rules in a chain, it helps to keep in mind how these rules are applied. You can see how things are laid out with a command like `iptables -L --line-numbers`. The `-L` flag lists the chains, and the `--line-numbers` flags includes the chain number for rules. Rules are sequentially in order. So if you have a rule that DROPS all traffic, you'll want to append that at the end of the chain, otherwise none of the other rules you write will be applied. Therefore if you want to make sure that a rule is applied, you will use an insertion method, but it can be safer to append rules that drop traffic.

The chains used for the filter table are `INPUT` chains, `FORWARD` chains, and `OUTPUT` chains. The `INPUT` chain handles incoming packets of traffic, the `FORWARD` chain handles packets that are being forwarded through the firewall to another network interface, and the `OUTPUT` chain handles outgoing packets of traffic. Other tables have their own kinds of chains, for example, the NAT table uses `PREROUTING` and `POSTROUTING` chains (and also has its own `OUTPUT` chain). The `PREROUTING`  and `POSTROUTING` chains are used for altering packets before and after routing respectively. You can also create user defined chains, but that won't be covered here.

When declaring a rule, in addition to specifying the chain, you are specify the target value (with the `-j` flag which is short for jump). The accepted target values are `ACCEPT`, `DROP`, `REJECT`, `LOG`, `QUEUE`, and `RETURN`. `ACCEPT` accepts packets, and `DROP` drops them, and are the two values you will principly be using. `REJECT` also discards matching packets, but also sends a response to the sender indicating that the packet was not delivered. `LOG` writes information about a received packet to the system. `QUEUE` is used to pass a packet to user-space for further processing. Finally, `RETURN` stops processing a packet and moves to the next rule in the chain.

`COMMIT` line in the iptables rules indicates the end of a set of rules and chains for your table, so you will see this line at the end of each table.

## Saving Changes

When using `iptables` CLI it does not automatically save or apply changes. The exact process varies depending on what flavor of Linux you are using, but typically you will be using `iptables-restore` and `iptables-save` to apply and save changes. If unsure, check the manpages for your system. There is also a package called `iptables-persistent` which automatically loads iptable rules.
