# 5. Segment Routing and Traffic Engineering

**Time:** 75 minutes.

**Objective:** Explain a PCC-initiated policy that the headend already owns, then create one PCE-initiated SR policy from CNC with your own name and color.

Use the policy type on the whiteboard. For this PTA lab that type is **SRv6** when the core is SRv6, and **SR-MPLS** when the core is SR-MPLS. The CNC pages are the same shape. Pick the tab that matches the whiteboard and stay on it.

## 5.1 Words you need

| Term | Meaning in this lab |
| --- | --- |
| PCC | The headend router. It may ask SR-PCE for a path, and it can own a policy that was configured on the router. |
| PCE | SR-PCE. It computes paths and can instantiate a policy on a headend through PCEP. |
| PCC-initiated | The policy configuration lives on the headend. CNC learns the policy from SR-PCE and draws it. |
| PCE-initiated | You create the policy in CNC. SR-PCE pushes it to the headend. The headend shows it as delegated to the PCE. |
| Color | A number that identifies the intent. Steering in chapter 6 binds a VPN to a color. |
| Candidate path | One way to reach the endpoint. Preference chooses among candidate paths. |

## 5.2 Find the PCC-initiated policy

The instructor prepares one PCC-initiated policy before class, on the headend, with a color outside the `200 + seat` range. You do not type that router configuration.

1. Go to **Services & Traffic Engineering > Traffic Engineering**.
2. Open the **SRv6** or **SR-MPLS** tab named on the whiteboard.
3. Filter or sort the policy table until you find the PCC-initiated policy. The initiation column says PCC.
4. Record headend, endpoint, color, preference, and the active candidate path.
5. Select the policy and show it on the map. Name two hops on the drawn path.
6. If the table has no PCC row, say so. The rest of this chapter still works. The live create is the PCE-initiated policy. Tell the instructor the prepared PCC policy is missing.

**Result:** You can point at a policy the router owns, or you have recorded that the prepared policy is absent.

On the headend console, if the instructor opens one, these commands support the same story. Read them. Leave configuration mode.

```text
show segment-routing traffic-eng policy
show segment-routing traffic-eng pcc ipv4 peer
```

On an IPv6-only SRv6 headend the peer command may be the IPv6 form:

```text
show segment-routing traffic-eng pcc ipv6 peer
```

Use the form the instructor shows. In the output, find the color of the PCC policy and the PCE peer address.

## 5.3 Create a PCE-initiated policy

Your policy name is `pta-pce-seat-<seat>`. Your color is `200 + seat`. Two seats that reuse a color will fight over steering in chapter 6.

1. Stay on **Services & Traffic Engineering > Traffic Engineering**, on the same SRv6 or SR-MPLS tab.
2. Start the create action and choose **PCE-initiated**.
3. Set:
    - Name: `pta-pce-seat-<seat>`
    - Headend: the headend from your lab-values sheet
    - Endpoint: the endpoint from your lab-values sheet
    - Color: `200 + seat`
    - Optimization objective: IGP metric, unless the instructor writes a different objective
4. Leave bandwidth, affinity, and disjoint constraints empty unless the instructor adds one constraint for the whole room.
5. Preview the path. You want a path that passes through the core, from your headend to your endpoint.
6. If the preview is empty, stop. An empty preview means SR-PCE has no topology or no SID reachability. Use the checkpoint page. Committing an empty path teaches the wrong lesson.
7. When the preview shows a path, provision the policy.
8. Return to the policy table. Your row shows initiation **PCE**, your color, and an operational state that is up.
9. Select the row and display the path on the map.

**Result:** A PCE-initiated policy exists for your seat, and the map draws it.

## 5.4 Confirm on the headend

Ask the instructor for a console on the headend, or watch the instructor's console.

```text
show segment-routing traffic-eng policy
```

Find `pta-pce-seat-<seat>`. In the same output, the PCC-initiated policy is still present. The difference is ownership: one policy is configured on the router, and the other was instantiated by the PCE.

If your policy is missing on the router and present in CNC as down, the usual causes are a PCEP session that is down, a color conflict, or a headend that cannot place the SIDs. Stay with the checkpoint page. Delete only your own down policy, and only when the instructor agrees.

## 5.5 Clean-up rule

Keep this policy until chapter 6 has steered a VPN onto its color. At the end of the day, delete only `pta-pce-seat-<seat>`. Leave the PCC-initiated policy in place for the next session.

## Done when

- You can say who owns the PCC-initiated policy and who owns yours.
- Your PCE-initiated policy is up, with your color, and it is drawn on the map.
- You saw both policies in `show segment-routing traffic-eng policy`, or you have a written reason the headend console was unavailable.
