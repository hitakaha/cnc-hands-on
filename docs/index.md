# Crosswork CNC Hands-on

This is the operator lab for Cisco Crosswork Network Controller (CNC) 7.2.1-SP. You will use the dCloud session from chapter 1: one CNC, a five-node IOS-XR lab, and a dedicated SR-PCE lab.

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

## Lab values

Copy this table into your notes. Fill hostnames and interfaces from the CML labs and from the instructor. Keep passwords out of the guide, out of chat, and out of screenshots you share.

| Item | Your value |
| --- | --- |
| CNC UI | `https://[2001:db8:134::200]:30603` (from chapter 1) |
| XR CML lab | `5-nodes_L3VPN 8201-32FH 25.3.1-ipv6` |
| PCE CML lab | `xrv9k-25.4.1-pce-for-CNC7.2-ipv6` |
| Headend PE | |
| Endpoint PE | |
| Attachment interfaces | |
| PCE-initiated policy name | `pta-pce-01` |
| Policy color | assigned by the instructor |
| L3VPN name | `pta-l3vpn-01` |
| VPWS name | `pta-vpws-01` |

## How to read this guide

From this folder, with the existing virtual environment:

```bash
source venv/bin/activate
mkdocs serve
```

Open the local URL MkDocs prints. `mkdocs build` writes the static site under `site/`.
