## LAN

- All hosts in a LAN can share the same physical communication media
- Each frame is delivered to every host
- If the host is not the intended recipient, it drops the frame
- Hosts in the same LAN can also be connected by switches

## Media Access Control Protocols(MAC)

- Problem: How do hosts acess broadcast media? How do they avoid collisions?

1. Channel partitioning protocols
   - Allocate I/N bandwidth to every host
   - Share channel efficiently and fairly at high load
   - Inefficient at low load where load = # of senders
   - Used for optical networks
2. Taking turns protocols
   - Pass a token around active hosts, a host can only send data if it has the token
   - More efficient at low loads since single node can use more than I/N bandwidth
   - Drawbacks: Overhead to acquire the token, vulnerable to failures(lost token or failed node)
3. Random Access
   - Efficient at low load since single node can fully utilize channel.
   - At high load, there is a collision overhead
   - Imagine you are in a crowded room:
   - Carrier sense(CS): Listen before speaking and don't interrupt
   - Collision detection(CD): if someone else starts talking at the same time, stop
   - Randomness: don't start talking right away

## Network Layer(3) - Connectivity

- Deliver packets to specified IP addresses across multiple datalink layers
- Wide Area Network: network that covers a vast area, ie city or state
- Router: Forward each packet received on an incoming link to an outgoing link based on packet destination IP address. _Forwarding Table_: mapping between IP address and output link

### Why should we use IP addresses instead of MAC addresses which are also unique?
- Doesn't scale. MAC addresses are like social security numbers, IP addresses are like unreadable home addresses
- MAC addresses are uniquely associated to the device for the entire lifetime of the device
- IP addresses change as the device location changes
- Since IP addresses usually specify where you are, we can keep a smaller networking table while we would need a huge one for MAC addresses.

- Internet Protocol(IP) is a best effort packet delivery
- Port Number: 16 bit number identifying the end point of a transport connection

## Drawbacks on layering
- Layer N may duplicate layer N-1 functionality
- Layers may need the same information
- Layering can hurt performance
- Layers are not cleanly separated
- Headers start to get really big(ie bigger than content)