# 6. Intent-based L3VPN provisioning

**Time:** 75 minutes.

**Objective:** Create one L3VPN from CNC. NSO renders the device configuration from the service model and the function-pack template. Steer the VPN onto the PCE-initiated policy from chapter 5.

The UI path is **Services & Traffic Engineering > Provisioning (NSO)**. CNC 7.1 provisions IETF L3VPN network models through NSO. You fill in the service intent. You do not paste CLI, and you do not edit the XML template on the NSO server.

## 6.1 What you are about to commit

Each seat creates a different service:

| Field | Value |
| --- | --- |
| Service name | `PTA-L3-SEAT-<seat>` |
| Endpoints | the two PEs on your lab-values sheet, using the attachment points the instructor assigns |
| Route distinguisher and route targets | the pair on the whiteboard for your seat |
| Steering | preferred path, bound to color `200 + seat` |
| Transport | the same SRv6 or SR-MPLS type you used in chapter 5 |

Preferred path is the steering method for this lab because the map can show the VPN sitting on the policy you just created. On-demand next hop (ODN) is the other method: the headend builds a path when it receives a route with that color. Read the ODN choice in the form so you recognize it, then select preferred path.

If the PTA service profile on this controller exposes only one steering knob, use the one the instructor names. Record which one you used.

## 6.2 Create the service

1. Go to **Services & Traffic Engineering > Provisioning (NSO)**.
2. Open the L3VPN service type.
3. Create a service named `PTA-L3-SEAT-<seat>`.
4. Select the instance profile or VPN profile the instructor named. That profile points at the NSO L3VPN template. You are choosing it, not editing it.
5. Enter the route distinguisher and route targets for your seat.
6. Add the two endpoints. For each endpoint set the device, the attachment circuit or interface, and the PE-CE routing the instructor specified. A lab that uses a static route on one side and BGP on the other is still one L3VPN. Follow the whiteboard.
7. Set the underlay transport to the policy type from chapter 5.
8. Set steering to preferred path and select color `200 + seat`, or select the policy `pta-pce-seat-<seat>` if the form asks for a policy name.
9. Run **Dry run** before **Commit**. Read the preview far enough to see your service name, your two endpoints, and the color. A preview that names another seat's VPN is the wrong form. Close it and start again from your own service name.
10. Commit.
11. Open the service and read the sync state. In sync means NSO and the devices agree. Out of sync belongs on the checkpoint page before you troubleshoot the VPN itself.

**Result:** `PTA-L3-SEAT-<seat>` is committed and in sync.

## 6.3 See it on the map

1. Open **Services & Traffic Engineering > VPN Services**, or the service overlay the provisioning page links to.
2. Select your VPN.
3. Show the service path and the underlay policy together.
4. Confirm the underlay is `pta-pce-seat-<seat>`, or a path whose color is `200 + seat`.

**Result:** You can show a neighbour the service and the SR policy it rides.

## 6.4 Read the effect on the PE

On the headend, with the instructor's console:

```text
show bgp vpnv4 unicast summary
show bgp vpnv6 unicast summary
```

Use the address family this lab actually runs. PTA's IPv6 core still may carry an IPv4 VPN, an IPv6 VPN, or both. The whiteboard says which summary command should show your VRF.

Then:

```text
show vrf
show segment-routing traffic-eng policy
```

You want the VRF for `PTA-L3-SEAT-<seat>` and the same PCE-initiated policy as chapter 5. Leave configuration mode.

## 6.5 Templates, stated once

The XML templates in the NSO L3VPN function pack are what turned your form into CLI. Device Management templates from chapter 4 did not. If a dry run fails with a template or profile error, the profile name is the thing to check. Editing the package on the NSO server is an instructor action outside this class.

## 6.6 Clean-up rule

Keep your VPN until you have shown it on the map. At the end of the day, delete `PTA-L3-SEAT-<seat>` from the provisioning page, then delete your PCE-initiated policy. Deleting the policy first, while the VPN still prefers it, leaves the service with nowhere to steer. Order is VPN, then policy.

## Done when

- Dry run showed your service name, your endpoints, and your color.
- The service is in sync.
- The VPN overlay draws your service on your PCE-initiated policy.
- You can say which template store rendered the CLI.
