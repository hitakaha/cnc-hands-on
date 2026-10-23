# 2. CNC management

**Time:** 40 minutes.

**Objective:** Decide whether the platform is healthy enough for topology, inventory, traffic engineering, and service provisioning. You are reading the system, not rebuilding it.

The three dependencies for the rest of the lab:

| Later work | Depends on |
| --- | --- |
| Topology and SR policies | SR-PCE provider, BGP-LS and PCEP from the routers |
| Inventory, alarms, interface utilization | Crosswork Data Gateway collection |
| L3VPN provisioning | NSO provider, devices in sync |

## 2.1 Application health

1. Go to **Administration > Crosswork Manager**.
2. Open the cluster or application view the page offers.
3. Find these applications and read the state next to each one:
    - Platform infrastructure
    - Optimization Engine
    - Active Topology
    - Element Management Functions / Device Lifecycle Management
    - Data Gateway related services
4. A state of Running or Healthy is what you want before chapter 3. Degraded and Down belong on the checkpoint page.

**Result:** You can name the applications this lab uses and their states.

Leave restart, node drain, and maintenance controls untouched. You already finished maintenance mode in chapter 1.

## 2.2 Providers

CNC learns the routed topology from SR-PCE. CNC provisions services through NSO. Both are providers.

1. Go to **Administration > Manage Provider Access**.
2. Find the SR-PCE provider. Read its reachability and the protocol state. You want the provider reachable, with the topology feed up.
3. Find the NSO provider. Read its reachability. You want NSO reachable.
4. Open the provider detail far enough to see the credential profile name. The profile is a pointer. The password stays in the profile store.

**Result:** SR-PCE and NSO are both reachable. If either is down, stop and use the checkpoint page before chapter 5 or chapter 6. Chapters 3 and 4 can still teach the screens.

## 2.3 Data Gateway

Interface counters, inventory detail, syslog, and SNMP traps arrive through a Crosswork Data Gateway (CDG). LCM in chapter 7 reads interface utilization from this collection, through SNMP.

1. Go to **Administration > Data Gateway Management**, or the Data Gateway page linked from Crosswork Manager.
2. Confirm the gateway is enrolled and the pool is up.
3. Open one attached device and confirm a data gateway is mapped to it. A device with no gateway mapping has no inventory refresh and no interface counters.
4. If a collection job or destination is listed, read its status. Healthy collection is the bar.

**Result:** You can point at the gateway that will feed alarms, inventory, and utilization.

## 2.4 What you leave alone

Credential profiles, AAA, certificates, backup jobs, and licensing are visible from Administration and from **Device Management > Credential Profiles**. Open a profile only if a device is unreachable and the instructor asks you to compare the profile name. Editing a profile on a shared controller locks every other seat out of that device.

Backup and restore put the cluster into maintenance mode. They are out of scope for this session.

## Done when

- Optimization Engine, Active Topology, and element management are Running or Healthy.
- The SR-PCE provider is reachable.
- The NSO provider is reachable.
- A Data Gateway is enrolled and mapped to the lab devices.
