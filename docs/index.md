# Crosswork CNC Hands-on

This is the operator lab for Cisco Crosswork Network Controller (CNC) 7.2.0. You will use the dCloud session from chapter 1: one CNC, a five-node IOS-XR lab, and a dedicated SR-PCE lab.

Each chapter has an objective, the clicks or CLI, the result you should see, and a short **Done when** list. Router work in this lab is `show` output unless a chapter says otherwise. Configuration changes go through CNC.

## Lab story

1. Bring CNC out of maintenance and start both CML labs.
2. Confirm CNC, the providers, and collection are healthy.
3. Read the topology in the UI and confirm it from XR CLI.
4. Walk inventory, faults, configuration, and performance, mapped to the SolarWinds views operations already uses.
5. Observe a PCC-initiated SR policy, then create a PCE-initiated SRv6 policy.
6. Provision an L3VPN and steer it onto that policy.
7. Provision a VPWS L2VPN. VPLS, an NSO custom template, and `sync-from` are optional.
8. Enable Service Health on the VPWS you created.
9. Collect showtech, read collection jobs and alarms, and walk the patch screen.
10. Optional: ask the CNC MCP assistant one inventory question.

