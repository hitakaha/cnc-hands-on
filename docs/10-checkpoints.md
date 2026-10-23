# Checkpoints

Use this page when a chapter's expected result does not appear. Fix the first row that matches, then return to the chapter. One person applies a shared fix. Everyone else waits.

## Maintenance and login

| What you see | What to check |
| --- | --- |
| The UI never finishes loading after the slider moves | Maintenance restart takes several minutes. Wait. If a popup asks to turn maintenance off, the instructor accepts it once. |
| Applications stay down after maintenance is Off | CNC 7.1: turn Maintenance On, wait until the mode is fully on, then turn it Off. Path: **Administration > Settings > System Settings > Maintenance Mode**. |
| Certificate warning | Confirm the hostname is the whiteboard URL, then use the lab exception the instructor shows. |

## CML and reachability

| What you see | What to check |
| --- | --- |
| Topology stays empty | CML nodes are booted. SR-PCE provider is reachable. BGP-LS from the IGP is up. Give a freshly started lab several minutes to advertise. |
| A PE is unreachable in Network Devices | The node is booted, the management address pings from the path CNC uses, and the credential profile name on the device is the lab profile. |
| Inventory is thin or old | The device has a Data Gateway mapping, and the gateway is enrolled. |

## Providers and collection

| What you see | What to check |
| --- | --- |
| SR-PCE provider down | Provider reachability in **Administration > Manage Provider Access**, then the PCE process on the CML node the instructor names. |
| NSO provider down | The same provider page. Chapter 6 waits until NSO is reachable. |
| Link utilization is blank | Data Gateway collection and SNMP credentials. LCM cannot see congestion without those counters. |
| Link utilization is 0% | Expected on an idle CML lab. Chapter 7 accepts that number. |

## SR policy

| What you see | What to check |
| --- | --- |
| No PCC-initiated row | The instructor's prepared policy is missing. Continue with the PCE-initiated create. |
| Path preview is empty | SR-PCE has no path in that policy type (SRv6 versus SR-MPLS). Confirm you are on the whiteboard's tab, then confirm SIDs on the topology layer. |
| Policy is down on the headend | PCEP session on the headend (`show segment-routing traffic-eng pcc ipv4 peer` or the IPv6 form). Color collision with another seat. |

## L3VPN

| What you see | What to check |
| --- | --- |
| Dry run names another seat | Close it. Reopen `PTA-L3-SEAT-<seat>` only. |
| Commit fails on the profile or template | The instance profile name on the whiteboard. The NSO L3VPN function pack is an instructor concern. |
| Service out of sync | NSO provider reachability, then the device sync state for your two PEs. Read sync before you debug the VRF. |
| Overlay does not show your policy | Steering color is `200 + seat`, and `pta-pce-seat-<seat>` is still up. |

## LCM

| What you see | What to check |
| --- | --- |
| No domain card | SR-PCE topology and BGP-LS `link-state instance-id`. The instructor configures LCM once. |
| No recommendation | Compare interface utilization with the interface threshold. Below the threshold, an empty dashboard is the correct result. |
| Many recommendations appear at once | Thresholds were lowered on an idle lab. The instructor owns the dashboard. Students do not commit. |

## Assistant

| What you see | What to check |
| --- | --- |
| Backend offline | The instructor restarts the assistant. Students wait. |
| Inventory tool returns an error | Reachability from the MCP server to the CNC URL, and the server's credential. That credential is not entered in chat. |
| The count disagrees with Network Devices | Write both numbers. Inventory counts modules as well as chassis on some views. Confirm you are comparing the same object type. |

## End of day

Delete in this order, and only your own objects:

1. L3VPN `PTA-L3-SEAT-<seat>`
2. PCE-initiated policy `pta-pce-seat-<seat>`

Leave the PCC-initiated policy, the LCM configuration, ZTP objects, templates, and providers in place.
