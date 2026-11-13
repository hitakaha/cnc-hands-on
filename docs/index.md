# Crosswork CNC Hands-on

This is the operator lab for Cisco Crosswork Network Controller (CNC) 7.2.0. You will use the dCloud session from chapter 1: one CNC, a five-node IOS-XR lab, and a dedicated SR-PCE lab.

Each chapter has an objective, the clicks or CLI, the result you should see, and a short **Done when** list. Router work in this lab is `show` output unless a chapter says otherwise. Configuration changes go through CNC.

## Lab story

1. Bring CNC out of maintenance and start both CML labs.
2. Confirm CNC, the providers, and collection are healthy.
3. Read the topology in the UI and confirm it from XR CLI.
4. Walk inventory, faults, configuration, and performance.
5. Create PCC-initiated SRv6 policies with PCE-delegated dynamic paths.
6. Provision an L3VPN.
7. Provision a L2VPN VPWS and check its Service Health.
8. Provision a L2VPN VPLS with multi-homing, steer it onto an SRv6 ODN policy, and check its Service Health.
9. Collect showtech, read collection jobs and alarms, and walk the patch screen.

