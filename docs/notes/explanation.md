# IPv4 Networking Notes (NetPractice)

## 0) Core Terms

- Bit: smallest unit, value `0` or `1`.
- Byte: 8 bits.
- Octet: 1 byte in an IPv4 address. IPv4 has 4 octets.
- IPv4 address: 32 bits, written as `a.b.c.d` (each octet from 0 to 255).
- Prefix: CIDR notation `/n`, where `n` is number of network bits.
- Subnet mask: 32-bit mask with network bits = `1`, host bits = `0`.
- Network part: bits that identify the subnet.
- Host part: bits that identify a host inside that subnet.
- Network address: first address of subnet (all host bits `0`).
- Broadcast address: last address of subnet (all host bits `1`).
- Usable host addresses: addresses between network and broadcast.
- Gateway (default route): router address used to reach other networks.
- Link: direct L2/L3 connection between two interfaces (for example router-to-router link).
- Subnet: logical network segment defined by mask/prefix.
- Address range: from network address to broadcast address.
- Step (block size): increment between subnet starts in an octet.

---

## 1) Octet Values and Prefix Logic

### Binary Weights in One Octet

`128 64 32 16 8 4 2 1`

Example:

- `224 = 128 + 64 + 32 = 11100000` (3 leading ones in this octet)

### Valid Mask Octet Values

Only these values can appear in a subnet mask octet:

- `0` (`00000000`) -> 0 one-bits
- `128` (`10000000`) -> 1 one-bit
- `192` (`11000000`) -> 2 one-bits
- `224` (`11100000`) -> 3 one-bits
- `240` (`11110000`) -> 4 one-bits
- `248` (`11111000`) -> 5 one-bits
- `252` (`11111100`) -> 6 one-bits
- `254` (`11111110`) -> 7 one-bits
- `255` (`11111111`) -> 8 one-bits

### Prefix from Mask (example)

Mask `255.255.255.224`:

- `255` -> 8 bits
- `255` -> 8 bits
- `255` -> 8 bits
- `224` -> 3 bits

Total prefix: `8 + 8 + 8 + 3 = /27`

### Same Value in Different Octets

- 1st octet `255` contributes to `/8`
- 2nd octet `255` contributes to `/16` total
- 3rd octet `255` contributes to `/24` total
- 4th octet `255` contributes to `/32` total

---

## 2) Host Bits

Formula:

- `host_bits = 32 - prefix`

Examples:

- `/24` -> `32 - 24 = 8` host bits
- `/27` -> `32 - 27 = 5` host bits
- `/30` -> `32 - 30 = 2` host bits

---

## 3) Total Number of Addresses

Formula:

- `total_addresses = 2^(host_bits)`

Examples:

- `/24`: host bits = 8 -> `2^8 = 256`
- `/27`: host bits = 5 -> `2^5 = 32`
- `/30`: host bits = 2 -> `2^2 = 4`

---

## 4) Usable Hosts

Classic formula (normal subnets):

- `usable_hosts = 2^(host_bits) - 2`

Why `-2`:

- one address is network
- one address is broadcast

Examples:

- `/24`: `256 - 2 = 254`
- `/27`: `32 - 2 = 30`
- `/30`: `4 - 2 = 2`

Special cases:

- `/31` is often used on point-to-point links (no broadcast in practice).
- `/32` is a single host route.

---

## 5) Step (Block Size)

Formula:

- `step = 256 - mask_octet`

Use it in the first octet where mask is not `255`.

Examples:

- `/27` -> mask `255.255.255.224` -> step `256 - 224 = 32`
- `/26` -> mask `255.255.255.192` -> step `256 - 192 = 64`
- `/28` -> mask `255.255.255.240` -> step `256 - 240 = 16`

---

## 6) Finding Subnet Ranges

Method:

1. Find interesting octet (first non-255 mask octet).
2. Compute step.
3. Write subnet starts: `0, step, 2*step, ...`.
4. Pick the interval containing your IP octet.

Example: IP `192.168.1.77/27`

- `/27` -> mask octet `224`, step `32`
- starts: `0, 32, 64, 96, 128, 160, 192, 224`
- `77` is in `[64..95]`

So:

- Network: `192.168.1.64`
- Broadcast: `192.168.1.95`
- Usable range: `192.168.1.65 - 192.168.1.94`

---

## 7) Determine Which Network an IP Belongs To

Two methods:

- Fast range method (using step, as above).
- Binary/bitwise method: `network = ip AND mask`.

Example:

- IP: `10.20.30.200`
- Mask: `255.255.255.192` (`/26`)
- Step in 4th octet: `64`
- Ranges: `0-63, 64-127, 128-191, 192-255`
- `200` is in `192-255`

Result:

- Network: `10.20.30.192/26`

---

## 8) Network and Broadcast Address

Rules:

- Network = all host bits `0`
- Broadcast = all host bits `1`

Example: `172.16.5.130/26`

- `/26` -> step `64`
- ranges in 4th octet: `0-63, 64-127, 128-191, 192-255`
- `130` is in `128-191`

Therefore:

- Network: `172.16.5.128`
- Broadcast: `172.16.5.191`
- Usable hosts: `172.16.5.129 - 172.16.5.190`

---

## Extra: Quick Checklist for NetPractice

1. Convert prefix <-> mask.
2. Identify interesting octet and step.
3. Find network/broadcast range.
4. Verify host IP is not network/broadcast.
5. Check host and gateway are in same subnet.
6. Check router interfaces and static routes.
7. Check reverse route exists (very common missing part).

---

## Mini Prefix Table

- `/24` -> `255.255.255.0` -> 256 total / 254 usable
- `/25` -> `255.255.255.128` -> 128 total / 126 usable
- `/26` -> `255.255.255.192` -> 64 total / 62 usable
- `/27` -> `255.255.255.224` -> 32 total / 30 usable
- `/28` -> `255.255.255.240` -> 16 total / 14 usable
- `/29` -> `255.255.255.248` -> 8 total / 6 usable
- `/30` -> `255.255.255.252` -> 4 total / 2 usable
- `/31` -> `255.255.255.254` -> 2 total / point-to-point use
- `/32` -> `255.255.255.255` -> 1 host route

---

## 9) IP Basics (Layer 3)

IP is responsible for logical addressing and routing between networks.

What IP does:

- adds source and destination IP addresses
- allows routers to forward packets between subnets
- provides best-effort delivery (no delivery guarantee)

Important IPv4 header fields (simplified):

- Source IP
- Destination IP
- TTL (decreases at each router hop)
- Protocol (for example TCP = 6, UDP = 17)

TTL note:

- each router decrements TTL by 1
- if TTL reaches 0, packet is dropped
- this prevents infinite routing loops

---

## 10) TCP Basics (Layer 4)

TCP is a connection-oriented transport protocol.

What TCP provides:

- reliable delivery (retransmissions)
- ordered delivery (sequence numbers)
- error checking (checksum)
- flow and congestion control

Common TCP fields:

- Source port
- Destination port
- Sequence number
- Acknowledgment number
- Flags (SYN, ACK, FIN, RST)

### 3-Way Handshake

1. Client -> Server: SYN
2. Server -> Client: SYN-ACK
3. Client -> Server: ACK

After this, data transfer starts.

### Connection Close (simplified)

- FIN/ACK exchange (or RST for abrupt close)

---

## 11) IP vs TCP (Quick Difference)

- IP decides where packet should go (addressing/routing).
- TCP decides how data is delivered reliably between two endpoints.

Short model:

- IP = path between hosts
- TCP = reliable conversation over that path

---

## 12) Ports and Services (Quick Reference)

- Port identifies service on a host.
- IP + port identifies endpoint socket (for example `192.168.1.10:80`).

Examples:

- HTTP: TCP/80
- HTTPS: TCP/443
- SSH: TCP/22

In NetPractice context:

- routing gets packet to the right host/IP
- TCP/port gets packet to the right application on that host
