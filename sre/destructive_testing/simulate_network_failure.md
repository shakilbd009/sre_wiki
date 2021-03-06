Simulating Network Failure
======

The idea here is to simulate a struggling network connection. This is useful in destructive testing mostly. You want to observe *how* the application fails, or as I like to say, "Determining what failure looks like."

## Drop n% of Incoming/Outgoing Packets (Packet Loss Simulation)
`iptables -A INPUT -m statistic --mode random --probability 0.1 -j DROP `
`iptables -A OUTPUT -m statistic --mode random --probability 0.1 -j DROP`

## Simulate Network Latency with loss and bandwidth limitations
`tc qdisc add dev eth0 root netem delay 250ms loss 10% rate 1mbps `

## Add jitter to latency
`tc qdisc add dev eth0 root netem delay 50ms 20ms `

## Distribution makes jitter more real
`tc qdisc add dev eth0 root netem delay 50ms 20ms distribution normal `

## Reorder, duplicate, and corrupt packets
`tc qdisc add dev eth0 root netem reorder 0.02 duplicate 0.05 corrupt 0.01 `
