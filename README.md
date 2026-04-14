*This project has been created as part of the 42 curriculum by icymust.*

# NetPractice

## Description

NetPractice is a 42 networking training project focused on IPv4 routing and subnetting.
The goal is to solve 10 progressive network scenarios by setting correct IP addresses,
subnet masks, default gateways, and static routes so that hosts can communicate.

This repository contains:

- solved/exported level configurations
- notes in English and Russian
- per-level explanations in `docs/`

## Instructions

### 1) Run the training interface

From the project root:

```bash
cd net_practice
./run.sh
```

Then open the local URL shown in the terminal (usually `http://localhost:<port>`).

### 2) Solve and export each level

For each level in the web interface:

1. Set IPs, masks, gateways, and routes.
2. Validate connectivity in the simulation.
3. Use the Export option in the interface to save the level configuration.

### 3) Submission requirement

You must place exactly 10 exported configuration files (one per level) at the repository root.

Expected set (example naming):

- `level1.json`
- `level2.json`
- `level3.json`
- `level4.json`
- `level5.json`
- `level6.json`
- `level7.json`
- `level8.json`
- `level9.json`
- `level10.json`

## Resources

### Networking concepts studied

- TCP/IP addressing
- IPv4 32-bit addressing model
- subnet masks and CIDR prefixes
- network, broadcast, and usable host ranges
- default gateways
- static routes and default routes (`0.0.0.0/0`)
- router vs switch roles
- OSI model basics (especially L2/L3/L4)
- basic TCP behavior (ports, reliability, handshake)

### References

- RFC 791 (Internet Protocol): https://www.rfc-editor.org/rfc/rfc791
- RFC 793 (TCP): https://www.rfc-editor.org/rfc/rfc793
- CIDR/Subnetting overview: https://www.cloudflare.com/learning/network-layer/what-is-cidr/
- Cisco subnetting basics: https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html

### AI usage disclosure

AI was used as a writing and structuring assistant for:

- organizing notes and explanations in `docs/notes/`
- formatting per-level markdown write-ups in `docs/level-*`
- drafting this README in clear technical English

All network values, route logic, and exported level solutions were reviewed and validated manually.
