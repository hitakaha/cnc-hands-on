# 8. Assurance

**Time:** 40 minutes.

**Objective:** Enable CNC Service Health on the VPWS from chapter 7 and read Basic monitoring until the health column has a real state. Do not monitor VPLS.

Service Health scores a provisioned VPN against heuristic rules. For L2VPN it looks at VPWS xconnect state, device CPU and memory, and, in Advanced mode, interfaces, PCEP, SR policy, and path reachability. This lab uses **Basic Monitoring** on `pta-vpws-01` only.

## 8.1 Find the VPWS

1. Go to **Services & Traffic Engineering > VPN Services**.
2. Select `pta-vpws-01`.
3. Confirm vpn-type is point-to-point / VPWS. If you also created `pta-vpls-01` in chapter 7, leave it unmonitored.
4. Confirm the service is in sync. Service Health on an out-of-sync service is not a useful baseline. Fix sync in chapter 7 before you start monitoring.

**Result:** You are looking at the VPWS, not the L3VPN and not a VPLS.

## 8.2 Start Basic monitoring

1. In the Actions menu for `pta-vpws-01`, choose **Start monitoring**.
2. Set monitoring level to **Basic Monitoring**. Do not choose Advanced unless the instructor wants the extra subservices and you have time to wait. Switching levels can take more than 15 minutes before symptoms appear.
3. Select the configuration profile the instructor names (often a Gold or Silver heuristic profile that is already loaded).
4. Start monitoring.
5. Wait. The Health column moves from Initiated to Good, Degraded, Down, or Not Monitoring.

**Result:** Health is no longer empty.

If Health stays on Initiated, collection for Service Health is still starting. Parameterized collection jobs of type service-based appear under **Administration > Collection Jobs**. You will read those jobs in chapter 9. Give Basic monitoring several minutes on this lab.

## 8.3 Read the health of the VPWS

1. Open `pta-vpws-01`.
2. Read the Health state and the list of subservices. For Basic L2VPN / VPWS, expect rules that cover:
    - VPWS xconnect / control-plane health
    - Device health (CPU, memory)
    - ODN or SR policy health, if the service is bound to a policy
3. Open one subservice. Read the metric that is Good or Degraded. Typical metrics are `xconnect.state`, attachment-circuit state, and pseudowire state.
4. If Health is Degraded or Down, do not clear anything yet. Note the symptom, then confirm it on the PE:

```text
show l2vpn xconnect
show l2vpn xconnect detail
```

A down attachment circuit on the router should match a down AC metric in Service Health. If CNC says Down and XR says Up, wait for the next collection cycle, then look at collection jobs in chapter 9.

5. On the map, with the VPWS selected, confirm the overlay still terminates on the two PEs you provisioned.

**Result:** You can point at the heuristic result and at the matching XR xconnect state.

## 8.4 What you do not do in this chapter

- Do not start monitoring on `pta-vpls-01` or on any multipoint L2VPN.
- Do not start monitoring on `pta-l3vpn-01` unless the instructor adds it as extra credit. The required assurance object is the VPWS.
- Do not switch this VPWS to Advanced Monitoring after Basic has already started, unless the instructor wants to show the delay.
- Do not stop monitoring until the instructor says the chapter is done. Stopping with "retain historical data" is available; use it only if you need to restart.

## Done when

- Service Health is running on `pta-vpws-01` at Basic Monitoring.
- You can name the Health state and one subservice or metric.
- That metric agrees with `show l2vpn xconnect` (or you have recorded a collection lag).
- No VPLS is being monitored.
