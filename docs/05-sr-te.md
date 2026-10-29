# 5. SR-TE and SRv6

**Time:** 60 minutes.

**Objective:** Explain a PCC-initiated SRv6 policy that the headend already owns, then create one PCE-initiated SRv6 policy from CNC.

This CML topology is IPv6. Stay on the **SRv6** tab in Traffic Engineering. Use SR-MPLS only if the instructor tells you the lab is not SRv6.

## 5.1 Words you need

| Term | Meaning in this lab |
| --- | --- |
| PCC | The headend router. It can own a locally configured policy, and it has a PCEP session to SR-PCE. |
| PCE | SR-PCE in the second CML lab. It computes paths and can instantiate a policy on a headend through PCEP. |
| PCC-initiated | The policy configuration lives on the headend. CNC learns it from SR-PCE and draws it. |
| PCE-initiated | You create the policy in CNC. SR-PCE pushes it to the headend. The headend shows it as delegated to the PCE. |
| Color | A number that identifies the intent. Chapter 6 binds an L3VPN to this color. |
| Candidate path | One way to reach the endpoint. Preference chooses among candidate paths. |

## 5.2 Find the PCC-initiated policy

The instructor may prepare one PCC-initiated policy on the headend before class. You do not type that router configuration.

1. Go to **Services & Traffic Engineering > Traffic Engineering**.
2. Open the **SRv6** tab.
3. Find a policy whose initiation column says PCC. If none exists, record that and continue. The live create in this chapter is the PCE-initiated policy.
4. Record headend, endpoint, color, preference, and the active candidate path.
5. Select the policy and show it on the map. Name two hops on the drawn path.

**Result:** You can point at a policy the router owns, or you have recorded that none was prepared.

On the headend console:

```text
show segment-routing traffic-eng policy
show segment-routing traffic-eng pcc ipv6 peer
```

Find the PCC policy color and the PCEP peer address. That peer should be the SR-PCE you checked in chapter 2.

On the SR-PCE, if the instructor opens it:

```text
show pce ipv6 peer
show pce ipv6 lsp
```

The PCC session should be up. The LSP list should include the PCC-initiated policy when one exists.

## 5.3 Create a PCE-initiated policy

Use name `pta-pce-01` and the color the instructor assigns. Do not reuse a color that already belongs to the PCC-initiated policy.

1. Stay on **Services & Traffic Engineering > Traffic Engineering**, **SRv6** tab.
2. Start the create action and choose **PCE-initiated**.
3. Set:
    - Name: `pta-pce-01`
    - Headend: the headend from chapter 3
    - Endpoint: the endpoint from chapter 3
    - Color: the instructor's value
    - Optimization objective: IGP metric, unless the instructor writes a different objective
4. Leave bandwidth, affinity, and disjoint constraints empty unless the instructor adds one constraint.
5. Preview the path. You want a path through the five-node core, from headend to endpoint.
6. If the preview is empty, stop. An empty preview means SR-PCE has no SRv6 topology or no SID reachability. Recheck chapter 2 and the CLI in section 5.2. Do not commit an empty path.
7. When the preview shows a path, provision the policy.
8. Return to the policy table. Your row shows initiation **PCE**, your color, and an operational state that is up.
9. Select the row and display the path on the map.

**Result:** A PCE-initiated SRv6 policy exists and the map draws it.

## 5.4 Confirm on the headend

```text
show segment-routing traffic-eng policy
show segment-routing traffic-eng policy name pta-pce-01
```

Find `pta-pce-01`. If a PCC-initiated policy was prepared, it is still present. The difference is ownership: one policy is configured on the router, the other was instantiated by the PCE.

If your policy is missing on the router and present in CNC as down, check:

```text
show segment-routing traffic-eng pcc ipv6 peer
```

A down PCEP session, a color conflict, or a headend that cannot place the SRv6 SIDs are the usual causes. Delete only `pta-pce-01`, and only if it is down and the instructor agrees.

## 5.5 Clean-up rule

Keep this policy until chapter 6 has steered an L3VPN onto its color. At the end of the day, delete the VPNs first, then delete `pta-pce-01`. Leave any PCC-initiated policy in place.

## Done when

- You can say who owns the PCC-initiated policy (if one exists) and who owns `pta-pce-01`.
- `pta-pce-01` is up, with the assigned color, and it is drawn on the map.
- `show segment-routing traffic-eng policy` on the headend lists your policy, and `show segment-routing traffic-eng pcc ipv6 peer` shows the PCE up.
