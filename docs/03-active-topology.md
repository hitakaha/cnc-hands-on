# 3. Active Topology (CAT)

**Time:** 45 minutes.

**Objective:** Use Crosswork Active Topology to answer where a device sits, how it is connected, and which layer you are looking at.

Active Topology is the map. Optimization Engine supplies the traffic-engineering view on top of the same topology. You will switch between those views so the two products stay distinct in your notes.

## 3.1 Open the map

1. From the main menu, open **Topology**.
2. Wait for the devices from the CML lab to appear. A topology that stays empty usually means the SR-PCE provider is up and BGP-LS is down, or the CML nodes are still booting. Check chapter 2 and the checkpoint page.
3. Switch between the logical map and the geographical map if both are offered. The logical map is the one you will use for the rest of the day.
4. Zoom to the two provider edges named on your lab-values sheet. Those are the headend and the endpoint for chapters 5 and 6.

**Result:** You can find both PEs and at least one path between them.

## 3.2 Read a device and a link

1. Select the headend. Open the details panel.
2. Record the hostname, the management address, and the reachability state.
3. Select a core link that sits on a path between the headend and the endpoint.
4. In the link details, record:
    - IGP metric
    - TE metric, if one is shown
    - Interface names on each side
    - Utilization, if a value is shown
    - Adjacency SID or SRv6 adjacency information, if the link shows it
5. Say out loud which value a PCE would minimize if the objective was IGP metric, and which value it would minimize if the objective was TE metric.

Utilization on a CML link is often 0%. Write down the number you actually see. Chapter 7 uses it.

**Result:** One device record and one link record in your notes.

## 3.3 Layers and filters

The map can show more than one underlay. PTA's core work is segment routing, with SRv6 as the policy type when the whiteboard says so.

1. Open the layer or protocol selector.
2. Turn on the IGP layer and confirm the links you care about are still there.
3. Turn on the SR-MPLS or SRv6 layer that matches the whiteboard. Adjacency and prefix SID information belongs to this layer.
4. If a Flex-Algo layer is present, note the algorithm id and leave it. Flex-Algo is a map fact in this lab, not a configuration task.
5. Apply a filter or device group that shows only the lab core, if a group already exists. Creating a new group is optional and is an instructor choice on a shared map.

**Result:** You can turn the TE layer on and still recognize the same two PEs.

## 3.4 Dashboard tiles

1. Return to the home page or the topology dashboard.
2. Read the tiles that summarize device reachability, link state, and policy count.
3. If a tile shows zero SR policies, that is acceptable before chapter 5. Write the number down so you can compare it after you create a PCE-initiated policy.

**Result:** You have a baseline count of policies and of reachable devices.

## Done when

- Both lab PEs are on the map and reachable.
- You recorded metric and utilization for one core link.
- You can show the IGP layer and the segment-routing layer.
- You know the baseline SR policy count.
