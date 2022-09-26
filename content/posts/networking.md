- comment code, future sections use past sections
- latency to a dst is your neighbor's latency to dst + your latency to neighbor
- use self.s_log()

# Confusing parts
- when to use split horizon, route poisoning, poison reverse
- Routing is rules used for forwarding packets: want full connectivity(everything conencted to everything else), minimized latency
- routing happens in control plane
- Link state vs distance vector

# Distance Vector
- Speaker: Every node tells its neighbors how far it is from every destination
- Listener: Each node uses the neighbor closest to each destination as its next hop for that dest

## Problems
- Missing entries: Link goes down, but other links don't know that so they continue routing throught that link
    - Cause: Advertisement gets dropped or destination is unreachable in the topology
    - Solutions: Retransmit adversisement every X seconds. If it is completely unreachable, you can't solve that with routing
- Looping, two nodes think that each node has the closest path
    - Causes: Two routers have accepted routes through each other. This usually happens when a link goes down.
    - Solution: This is where split horizon and posion reverse
- Stale network info: entry in a forwarding table that no longer exists in reality
    - Any change in the network state causes stale network state
    - Additive info: propogates as quickly as the advertisement frequency - a new router comes up, new link gets added, new host attaches
    - Subtractive ino: as quickly as ttl of entries stored in the table - a router crashes, link goes down, hosts detach
    - Solution: Poison reverse

### Split Horizon
If I route through you, I will not tell you about my path
- Can't have a direct loop if one side never tells the other side about the route

### Poison Reverse
If I route through you, I will lie and tell you my cost to the dest is infinity
- All routers agree not to use paths that have cost infinity
- Trigger: Receiving an advertisement that causes you to go through a new next hop to some destiantation
- Problem: loop prevention and getting out of loops quickly
- Mechanism: send only the neighbor you use as your next hop
GENERALLY BETTER THAN SPLIT HORIZON IN ALL CASES, THE ONLY PROBLEM IS THAT IT TALKS A BIT MORE

### Route poisoning
Key observation we saw with poison reverse - you can override neighbor ttl period by sending adverstiment with infinite cost.
- When you lose a route, instead of deleting it from your forwading table, tell all your neighbors your route now has cost infinity.

## FAQ
- Can't use split horizon and poison reverse at the same time
- Pick either 1 or 0 from each: {Poison reverse, split horizon, nothing}, {route poisoning, nothing}

## Routing in L2 vs L3

### l2
Goals: Short paths, resilient to failure, oath for all destinations in a local network. Plug and play
Properties: small entwork, low cost
Solutions: learning switches and stp

### L3
Goals: short paths, resilient to failure, paths for all destinations in a potentially large network
Properties: Must be scalable: get a new address when you join a new network, allows routing to group based cidr prefixes

