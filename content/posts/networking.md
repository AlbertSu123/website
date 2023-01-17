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

# Midterm review

## Endhosts

## Links
- Properties that characterize a link: Bandwidth, propogration delay, bandwidth delay product
- Types of delays: transmission time, propogration time, queueing delay

## Routers
- Routers connect links, what do routers look like internally?
- Route processors vs linecards
- Control vs data plane
- Fast path vs slow path
- Key challenege: build a dataplane with high performance

### Routing
- Valid routes: no deadends or loops
- Link costs and least cost paths
- Route convergence 
- Achieve forwarding through default routes or static routes(how does a router know where a destination is?)

### Routing Algorithms
- Distance Vector, Link State, Spanning Tree Protocol

### Distance Vector
- I tell my neighbors about my least cost distance to a destination, they update their least cost distances and next hop choices accordingly
- Where we advertise routes
- Logic/rules for when to update a route
- Periodic vs triggered updates
- Understand what happens when routers fail, links fail, advertisements are dropped
- Counting to inifinty problem
- How TTL and advertisement rules can lead to long convergence times
- Split horizon: Don't tell your next hop anything
- Poison reverse: tell your neighbor the next hop is inifinity
- Poisoning a route: tell all your neighbors inifinity
- Distance Vector: Global computation(distributed across all ndoes) using local data(from just itself and its neighbors)

### Link State
- Every router
    - Gets the state of all links and locations of all destinations
    - Uses that global information to build a full graph
    - Find paths from itself to every destination on a graph
    - Use the second hop in those paths to populat its forwarding table
- Link state: Local computation using global data(from all parts of the network)

### Learning Switches and Spanning Tree Protocol
- Idea: Learn routes from watching where data packets come from, if you don't have a route, just flood(flooding -> need loop-free topology -> use spanning tree protocol)
- Know how we discover the root and next hop to root
- Know how we disable links not on path to the root
- Pros: Enables plug and play hosts. Cons: Disabling links is wasteful

## Interdomain routing
- Distance Vector, Link State, and Spanning Tree Protocol are for routers within a domain, what about routers outside the domain

### Domains(Autonomous Systems)
- Types of AS: transit vs stub, tier 1 transit providers(who are they, they don't have providers)
- Understand business relationships between AS - customer-provider vs peer to peer
- Customers don't pay providers, peers don't pay each other
- An AS connects to other AS as a customer/provider/peer
- An AS obtains a prefix used to represent all hosts within an AS
- Goal: ASes pick routes based on policy while preserving autonomy and privacy
- Typical policy: Make money/save money

## Addressing: hierarchical
- Addresses structured as network prefix and host suffix
- Remember address allocation is hierarchical: ICANN-> RIR -> AT&T -> UCB
- Interdomain roting works on prefixes, which can be aggregagted
- Destinations match using longest prefix, not exact match

### Policy based routing and GAO Rexford
- Advertise entire path vector
- Aggregate prefixes as you go
- Don't have to advertise a route to all prefix
- Not always least cost
- Gao-Rexford: capture common practice for export/selection rules
- Gao Rexford chooses customer > peer > provider

## How does BGP work?
- eBGP, iBGP, IGP
- Route advertisements = <prefix, attributes>
- attributes include ASPATH, Local pref, MED, IGP

## Statistical Multiplexing
- Sharing approaches
    - Reservations: End hosts explicity reserve bandwidth for some time
    - Best effort: Send data packets when you have them and hope for the best
- Canonical designs for sharing appraoches
    - reservations via circuit switching
    - Best effort via packet switching(What the internet uses)

### Circuit switching vs Packet switching
- Pros of circuit switching:
    - Better application performance(reserved bandwidth)
    - More predictable and understandable(w/o failures)
- Pros for packet switching
    - Better efficiency
    - Faster startup to first packet delivered
    - Easier recovery from failure
    - Simpler implementation

#