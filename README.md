# Cisco Router-on-a-Stick Lab

A hands-on Cisco networking lab demonstrating VLAN segmentation, 802.1Q trunking, and inter-VLAN routing using the Router-on-a-Stick architecture.

## Overview

This project demonstrates how multiple VLANs can communicate through a single physical router interface using 802.1Q trunking and router subinterfaces.

The lab was built and tested using Cisco Packet Tracer.

## Network Topology

![Network Topology](/topology/topology.png)

## Network Architecture

The topology consists of:

- 1 Cisco ISR4300 router
- 2 Cisco Catalyst 2960 switches
- 2 end devices
- 2 VLANs
- 802.1Q trunk links
- Router-on-a-Stick inter-VLAN routing

### Physical Connections

| Device | Interface | Connected To | Purpose |
|---|---|---|---|
| Router0 | Gi0/0/0 | S1 Fa0/1 | 802.1Q trunk |
| S1 | Fa0/2 | S2 Fa0/1 | 802.1Q trunk |
| S1 | Fa0/3 | PC1 | VLAN 10 access |
| S2 | Fa0/2 | PC2 | VLAN 20 access |

## VLAN and IP Addressing

| VLAN | Name | Network | Default Gateway | Host |
|---:|---|---|---|---|
| 10 | VLAN10 | 192.168.10.0/24 | 192.168.10.1 | PC1 |
| 20 | VLAN20 | 192.168.20.0/24 | 192.168.20.1 | PC2 |

### End Devices

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

## How Router-on-a-Stick Works

Router-on-a-Stick is a way to allow different VLANs to communicate with each other using one physical interface on the router. In this lab, the router is connected to S1 through Gi0/0/0, and this connection works as a trunk, allowing traffic from VLAN 10 and VLAN 20 to pass through the same link.

To build this lab, I first created VLAN 10 and VLAN 20 on both switches. PC1 was connected to S1 on Fa0/3, so I assigned this port to VLAN 10. PC2 was connected to S2 on Fa0/2, so I assigned this port to VLAN 20.

After that, I configured the connections between the switches as trunk ports. This is important because the link between S1 and S2 needs to carry both VLANs. I also configured the connection between S1 and the router as a trunk, so the router can receive traffic from both VLANs.

On the router, I created two subinterfaces under Gi0/0/0. The first one was Gi0/0/0.10 for VLAN 10 and the second one was Gi0/0/0.20 for VLAN 20. Each subinterface was configured with 802.1Q using the VLAN number and was given an IP address to act as the gateway for that VLAN.

For VLAN 10, the gateway is 192.168.10.1, and for VLAN 20, the gateway is 192.168.20.1. PC1 uses 192.168.10.1 as its default gateway and PC2 uses 192.168.20.1.

After configuring everything, I tested the connection between the PCs. PC1 was able to communicate with PC2 even though they are in different VLANs. This confirmed that the router was correctly routing the traffic between VLAN 10 and VLAN 20.
