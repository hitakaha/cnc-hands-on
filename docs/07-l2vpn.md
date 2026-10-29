# 7. L2VPN provisioning

**Time:** 60 minutes for VPWS. Optional sections add about 30 minutes.

**Objective:** Provision a point-to-point L2VPN (VPWS) from CNC. Optionally inspect VPLS, attach an NSO custom template, and run `sync-from`.

CNC 7.2 provisions IETF L2VPN network models through NSO. VPWS is a wire between two attachment circuits. VPLS is a multipoint LAN. This lab's required service is VPWS. Chapter 8 monitors that VPWS only.

## 7.1 VPWS (required)

| Field | Value |
| --- | --- |
| Service name | `pta-vpws-01` |
| vpn-type | point-to-point / VPWS (use the value the form offers for two endpoints) |
| Endpoints | the two PEs from chapter 3, on the L2 attachment interfaces the instructor assigns |
| Transport | SRv6, preferably the same color or policy as `pta-pce-01` if the form allows it |

Do not reuse the L3VPN attachment interfaces from chapter 6. VPWS needs its own attachment circuits.

1. Go to **Services & Traffic Engineering > Provisioning (NSO)**.
2. Open the L2VPN service type.
3. Create a service named `pta-vpws-01`.
4. Set **vpn-id** to a unique value the instructor assigns.
5. Set **vpn-type** to the point-to-point / VPWS choice. Do not choose a multipoint or VPLS type here. If the form only lists `mpls-evpn` when you add more than two vpn-nodes, you have the wrong number of endpoints. VPWS uses two.
6. Add two vpn-nodes. For each node set the device and the attachment circuit (interface, VLAN if the instructor specified one).
7. Set underlay transport to SRv6. If preferred-path or ODN is offered, bind it the way the instructor specified. Preferred path onto `pta-pce-01` is the consistent choice with chapter 6.
8. Leave **Custom-template** empty for the required service. Optional section 7.3 uses that field on a copy, not on this one.
9. Run **Dry run**. Confirm the service name, two endpoints, and vpn-type. Confirm it is not a VPLS.
10. Commit.
11. Open the service. Confirm it is in sync.

**Result:** `pta-vpws-01` is committed and in sync.

## 7.2 Visualize and confirm from CLI

1. Go to **Services & Traffic Engineering > VPN Services**.
2. Select `pta-vpws-01`. Show it on the map. You should see a point-to-point overlay between the two PEs, not a multipoint LAN.
3. On the headend:

```text
show l2vpn xconnect
show l2vpn xconnect detail
```

Find the xconnect NSO created. Attachment circuit and pseudowire (or SRv6 EVPN VPWS equivalent) should be up. If the lab uses EVPN VPWS, the instructor may also ask for:

```text
show l2vpn evpn xconnect
show bgp l2vpn evpn summary
```

Use the form that actually prints your service. Leave configuration mode.

**Result:** The map is point-to-point, and XR shows the xconnect up.

## 7.3 Optional: VPLS

Do this section only if the instructor asks for it. Chapter 8 does **not** enable Service Health on a VPLS.

1. In **Provisioning (NSO)**, create a second L2VPN named `pta-vpls-01`.
2. Choose the VPLS or multipoint vpn-type the form offers. Add at least two vpn-nodes (three if the instructor wants a true multipoint picture).
3. Dry run, then commit.
4. In **VPN Services**, select `pta-vpls-01` and confirm the overlay is multipoint.
5. On a PE:

```text
show l2vpn bridge-domain
```

Use this only to contrast with `show l2vpn xconnect` from the VPWS. Delete `pta-vpls-01` before the end of the day if you created it. Do not start Service Health on it.

**Result:** You can tell VPWS (xconnect) from VPLS (bridge-domain) in both CNC and CLI.

## 7.4 Optional: NSO custom template

An NSO custom template adds CLI that the L2VPN function pack does not model (for example an extra QoS or interface command). Templates must already exist in NSO. CNC only displays parameterizable fields.

1. Ask the instructor for the template name, if one is loaded.
2. Open `pta-vpws-01` in edit, or clone it to `pta-vpws-template-01` if you should not change the monitored service.
3. In **Custom-template**, select the template. Fill the variables CNC shows.
4. Dry run. Read the extra CLI the template adds.
5. Commit only if the instructor wants the extra CLI on the device.
6. If you only needed to see the fields, cancel without commit.

This is not a Device Management configuration template from chapter 4.

**Result:** You saw how a custom template attaches to a VPWS, and you know which store it lives in.

## 7.5 Optional: sync-from

NSO keeps a copy of device configuration. **Check sync** compares that copy with the device. **Sync from** pulls the device configuration into NSO. **Sync to** pushes NSO to the device. **Redeploy reconcile** rebuilds the service while keeping non-service configuration.

Use this on the VPWS only when the instructor wants the room to see out-of-sync handling. `sync-from` after a service commit can make the service look out of sync until you redeploy. That is the lesson, and it is easy to leave messy.

1. On `pta-vpws-01`, open **Actions**.
2. Run **Check sync**. Record in-sync or out-of-sync.
3. Open **Config view** or **View in NSO** and read the service data.
4. If the instructor has made a small out-of-band change on one PE, run **Sync from**.
5. Run **Check sync** again. Expect out-of-sync if the device now differs from the service model.
6. Run **Redeploy dry run**, then **Redeploy reconcile** only if the instructor wants the service model to win.

Do not run **Sync to** unless the instructor says so. Sync to overwrites the device with NSO's copy.

**Result:** You can say what Check sync, Sync from, Sync to, and Redeploy reconcile each do, and you used only the actions the instructor approved.

## 7.6 Clean-up rule

Keep `pta-vpws-01` through chapter 8. Delete optional VPLS and template clones first at the end of the day, then the VPWS, then the L3VPN, then the PCE-initiated policy.

## Done when

- `pta-vpws-01` is in sync and shown as point-to-point in VPN Services.
- `show l2vpn xconnect` (or the EVPN form) lists the service.
- Optional sections are either completed or marked skipped.
