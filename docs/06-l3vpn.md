# 6. L3VPN provisioning

**Time:** 60 minutes.

**Objective:** Create one L3VPN from CNC. NSO renders the device configuration from the IETF L3VPN network model. Steer the VPN onto the PCE-initiated SRv6 policy from chapter 5.

The UI path is **Services & Traffic Engineering > Provisioning (NSO)**. You fill in the service intent. You do not paste CLI, and you do not edit the XML template on the NSO server.

## 6.1 What you are about to commit

| Field | Value |
| --- | --- |
| Service name | `l3vpn-01` |
| Endpoints | the two PEs from chapter 3, using the attachment points the instructor assigns |
| Route distinguisher and route targets | assigned by the instructor |
| Steering | preferred path, bound to the color of `pce-01` |
| Transport | SRv6 |

Preferred path is the steering method for this lab because the map can show the VPN sitting on the policy you created. On-demand next hop (ODN) is the other method: the headend builds a path when it receives a route with that color. Read the ODN choice in the form so you recognize it, then select preferred path unless the instructor names a different knob.

## 6.2 Create the service

1. Go to **Services & Traffic Engineering > Provisioning (NSO)**.
2. Open the L3VPN service type.
3. Create a service named `l3vpn-01`.
4. Select the instance profile or VPN profile the instructor named. That profile points at the NSO L3VPN template. You are choosing it, not editing it.
5. Enter the route distinguisher and route targets for this lab.
6. Add the two endpoints. For each endpoint set the device, the attachment circuit or interface, and the PE-CE routing the instructor specified. A lab that uses a static route on one side and BGP on the other is still one L3VPN.
7. Set the underlay transport to SRv6.
8. Set steering to preferred path and select the color of `pce-01`, or select the policy name if the form asks for a policy.
9. Run **Dry run** before **Commit**. Read the preview far enough to see your service name, your two endpoints, and the color.
10. Commit.
11. Open the service and read the sync state. **In sync** means NSO and the devices agree. Out of sync: use **Check sync** before you debug the VRF. `Sync from` is an optional exercise in chapter 7, not the first response here.

**Result:** `l3vpn-01` is committed and in sync.

## 6.3 See it on the map

1. Open **Services & Traffic Engineering > VPN Services**.
2. Select `l3vpn-01`.
3. Show the service path and the underlay policy together.
4. Confirm the underlay is `pce-01`, or a path whose color matches that policy.

**Result:** You can show the L3VPN and the SRv6 policy it rides.

## 6.4 Read the effect on the PE

On the headend:

```text
show vrf all
show bgp vpnv6 unicast summary
show bgp vpnv4 unicast summary
```

This core is IPv6. The VPN address family may still be VPNv4, VPNv6, or both. Use the family the instructor named. Then:

```text
show segment-routing traffic-eng policy
```

You want a VRF for `l3vpn-01` (or the VRF name NSO assigned) and the same PCE-initiated policy as chapter 5. Leave configuration mode.

## 6.5 Templates, stated once

The XML templates in the NSO L3VPN function pack turned your form into CLI. Device Management templates from chapter 4 did not. If a dry run fails with a template or profile error, the profile name is the thing to check.

## 6.6 Clean-up rule

Keep this VPN until you have shown it on the map. Delete order at the end of the day is L2VPN, then L3VPN, then `pce-01`. Deleting the policy first, while a VPN still prefers it, leaves the service with nowhere to steer.

## Done when

- Dry run showed your service name, your endpoints, and your color.
- The service is in sync.
- The VPN overlay draws `l3vpn-01` on `pce-01`.
- `show vrf all` on the PE lists the VRF NSO created.
