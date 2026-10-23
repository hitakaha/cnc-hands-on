# 7. Closed-loop congestion mitigation

**Time:** 45 minutes.

**Objective:** Read Local Congestion Mitigation (LCM) on CNC 7.1, explain the closed loop, and leave the shared network in Manual mode.

## 7.1 Lab decision

LCM is a real Optimization Engine feature in CNC 7.1, and it is the right exercise for this chapter. It looks for congestion on a timer, estimates traffic that can be moved off a hot interface, and recommends tactical traffic-engineering (TTE) SR policies around that interface. In Manual mode an operator previews the recommendation and then commits it. SR-PCE deploys the tactical policies. When the interface cools, LCM recommends removal.

LCM sees congestion through SNMP interface counters collected by the Data Gateway. It also needs a discovered IGP domain (the BGP-LS `link-state instance-id`), an alternate local path, and a tactical color that no other policy uses.

A CML topology often reports **0% utilization**, because the links carry no traffic. LCM then has nothing to mitigate. That outcome still completes this chapter. Record the utilization and the threshold, and explain why the operational dashboard has no recommendation.

!!! note "Instructor"
    Keep the domain in **Manual** mode. Automated mode deploys tactical policies without a human commit, and in later releases it also requires a fully gRPC-compliant domain. Students do not change the mode.

    Commit a recommendation only when one domain shows a real recommendation and you want the room to see a single deploy. Use the reserved tactical color. Students do not click **Commit all**.

    If you want a live PCE action and every interface is idle, use Bandwidth on Demand as a demonstration. BWoD asks for a path with a bandwidth number. It does not need a hot link. It is a different feature. Show it from the instructor console, and keep the student exercise on the LCM screens.

## 7.2 Open the domain

1. Go to **Services & Traffic Engineering > Local Congestion Mitigation**.
2. Read the domain card. The domain id comes from the IGP `link-state instance-id` advertised in BGP-LS.
3. Record:
    - LCM status (enabled or not)
    - Operation mode
    - Urgency
    - The description, which defaults to "LCM Startup Config" on a fresh configuration
4. If the card says the domain is not configured, stop and let the instructor open **Configure**. The room uses one LCM configuration.

**Result:** You know the domain id and the operation mode. The mode you want to see is Manual.

## 7.3 Read the thresholds and the loop

1. Open **Interface Thresholds**. Find one core interface you recorded in chapter 3. Read its utilization threshold.
2. Compare that threshold with the utilization you wrote down from the topology map. Say which side of the threshold the interface is on.
3. Open **Configuration**. Read the congestion check interval, the optimization objective, and the operation mode. Leave every field as you found it.
4. Walk the loop in your own words:
    - CDG polls interface counters with SNMP.
    - LCM compares utilization with the interface threshold on the check interval.
    - A hot interface produces a TTE SR policy recommendation that stays local to that hop.
    - In Manual mode the recommendation waits on the operational dashboard.
    - Commit sends the tactical policy through SR-PCE.
    - A later check recommends deletion when the interface is under the threshold again.

**Result:** You can teach the loop from the screens, with your own utilization number in the story.

## 7.4 Recommendations

1. Open the **Operational Dashboard** for the domain. Use the "Recommendations available" link when the card shows it.
2. If a recommendation is listed, read urgency and the interfaces it would bypass. Preview it on the map. Then stop. The instructor commits, or nobody commits.
3. If the dashboard is empty, write the sentence: "No recommendation, utilization is below the threshold." Include the two numbers.
4. Open **Operational History** if it has rows from an earlier demo, and read one commit or one removal. History is part of the product even when this hour adds no new row.

**Result:** You have either previewed one recommendation or recorded why none exists.

## 7.5 What you leave alone

- The operation-mode control stays on Manual.
- **Commit all** stays with the instructor.
- The tactical color stays the reserved color, and it stays off your chapter 5 policy.
- Per-interface threshold edits are an instructor tool for a demo. A room full of lowered thresholds creates a pile of recommendations against empty CML links.

## Done when

- You can open the LCM domain, the thresholds, and the configuration.
- You compared one interface's utilization with its threshold.
- You can describe what a commit would deploy, and you did not deploy it yourself.
- You know BWoD is the instructor's fallback when the lab has no congestion to show.
