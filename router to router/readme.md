# Router-to-Router Lab Notes

## Topology



```
PC1 -- Switch1 -- Router1 -- Router2 -- Switch2 -- PC2
   192.168.1.0/24                    192.168.2.0/24
                  10.0.0.0/30 (link between routers)
```

- PC1: 192.168.1.2, Gateway: 192.168.1.1
- PC2: 192.168.2.2, Gateway: 192.168.2.1
- Router1: 192.168.1.1 (LAN), 10.0.0.1 (link to R2)
- Router2: 192.168.2.1 (LAN), 10.0.0.2 (link to R1)

## Things that ALL have to be correct for PC1 <-> PC2 to work

1. **PC gateways set correctly**
   - PC1 gateway = 192.168.1.1
   - PC2 gateway = 192.168.2.1
   - Without this, the PC doesn't know where to send traffic leaving its own subnet.

2. **Static route on Router1**
   ```
   ip route 192.168.2.0 255.255.255.0 10.0.0.2
   ```
   Tells R1: "to reach PC2's network, send it to R2."

3. **Static route on Router2**
   ```
   ip route 192.168.1.0 255.255.255.0 10.0.0.1
   ```
   Tells R2: "to reach PC1's network, send it to R1."

Miss any one of these 4 things and the ping breaks somewhere.

## Key terms

- **Trunk port**: only needed when a single link must carry multiple VLANs tagged together. No VLANs = no trunk needed.
- **Hop**: one router along the path a packet travels through.
- **Next-hop IP**: the IP of the very next router to forward a packet to (not the final destination).
- **Static route**: manually typed route, doesn't update itself.
- **Dynamic routing (OSPF/RIP/EIGRP)**: routers automatically share routes with each other, no manual `ip route` needed. Better for larger/growing networks.
- **DHCP**: auto-assigns IP addresses to devices. Unrelated to routing.
- **DNS**: translates names (google.com) to IPs. Unrelated to routing.

## Useful commands

```
ip route [destination network] [subnet mask] [next-hop IP]      -> add static route
no ip route [destination network] [subnet mask] [next-hop IP]   -> remove static route
show ip route                                                     -> view routing table
ping [ip]                                                         -> test connectivity
```

## Debugging order when a ping fails

1. PC -> its own gateway
2. Router -> next router (direct link)
3. PC -> far router's near-side interface
4. PC -> far router's LAN-side interface
5. Far PC -> its own gateway (check this is set!)

Whichever step fails first tells you exactly where the problem is.

