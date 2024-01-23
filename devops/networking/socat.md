# socat

`socat` sets up socket relays. You can get a list of address types and valid groups with `socat -h`.

For example, to map an existing port to a new port `socat TCP_LISTEN:<new port>,fork,reuseaddr TCP:<ip_addr:port>`.
