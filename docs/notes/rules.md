# NetPractice Rules And Practical Notes

## 1) No Same Subnet Around One Router On Different Interfaces

Different router interfaces should belong to different subnets.

Why:

- a router is made to route between different networks
- same-subnet interfaces create ambiguity and bad design
- can cause connected route conflicts

Rule: one interface = one subnet, no overlaps.

---

## 2) What Not To Use In Internet Routes

In Internet-facing tasks, do not use as next hop:

- 127.0.0.0/8 (loopback)
- private ranges 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 when the Internet side is public in the exercise

Why:

- next hop must be reachable on the actual external segment
- loopback is local-only
- private space is typically invalid in these Internet-side blocks

---

## 3) How To Read A Route (Left Vs Right)

General format:

destination -> next hop

Where:

- left side destination: target network or host
- right side next hop: where to send packets next

Examples:

- 13.38.0.0/16 -> 192.168.1.1
- 0.0.0.0/0 -> 163.172.250.1

Meaning:

- to reach 13.38.0.0/16, forward to 192.168.1.1
- if nothing else matches, use default via 163.172.250.1

---

## 4) Route To One Specific Host

Use a host route with /32:

- 13.168.0.1/32 -> 163.172.250.12

This route applies to one IP only.

---

## 5) Why /30 Is Usually For Links, Not LANs

/30 gives 4 addresses total:

- 1 network
- 2 usable
- 1 broadcast

Perfect for router-to-router point-to-point links (2 devices).

Poor choice for LAN with multiple hosts:

- only 2 usable addresses
- no room to grow

Conclusion:

- use /30 for transit links
- use /29, /28, /27, etc. for host LANs

---

## 6) Router Vs Switch

Router:

- connects different subnets
- forwards traffic using routing table
- usually has interfaces in different networks

Switch:

- connects devices inside one Layer-2 segment
- does not route between IP subnets
- hosts behind it are usually in one subnet

Short version:

- Router = between networks
- Switch = inside one network

---

## 7) Network Vs Subnet

Network:

- a larger address block

Subnet:

- a smaller block carved from a network using mask/prefix

Example:

- network 192.168.0.0/16
- subnets 192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24

---

## 8) Default Route

Default route:

- 0.0.0.0/0 -> next hop

Meaning:

- when no more specific route exists, use this path

Important:

- default should not break local routing
- longest prefix match always wins over default

---

## Extra Useful Rules

## 9) Host Gateway Must Be In Same Subnet

If host is 10.0.0.10/24, gateway should be 10.0.0.x (and not network/broadcast address).

---

## 10) Never Assign Network Or Broadcast To A Host

Example for /25:

- network: .0
- broadcast: .127

Usable hosts are only between them.

---

## 11) Avoid Overlapping Subnets

Overlaps create ambiguous routing and unstable behavior.

---

## 12) Always Validate Reverse Path

Forward path without return route means broken communication.

Common NetPractice issue:

- packets leave but replies never come back.

---

## 13) Route Priority Rule

Most specific route (longest prefix) is selected first.

Example:

- both 0.0.0.0/0 and 13.38.0.0/16 exist
- destination 13.38.x.x uses /16 route

---

## Quick Pre-Submission Checklist

1. Every interface has valid IP and mask.
2. No host uses network or broadcast address.
3. Each host gateway is in the same subnet.
4. Router-to-router links usually use /30.
5. Required static routes are present.
6. Default route exists where needed.
7. Reverse route exists for replies.
