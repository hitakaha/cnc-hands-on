# 2. CNC management

**Time:** 30 minutes.

**Objective:** Confirm that CNC 7.2 is healthy enough for topology, inventory, traffic engineering, and service provisioning. You are reading the system, not rebuilding it.

The rest of the lab depends on three things:

| Later work | Depends on |
| --- | --- |
| Topology and SR policies | SR-PCE provider, BGP-LS and PCEP from the routers |
| Inventory, alarms, interface counters | Data Gateway collection jobs |
| L3VPN and L2VPN provisioning | NSO provider, devices in sync |

## 2.1 Crosswork Summary and Crosswork Health

1. From the main menu, go to **Administration > Crosswork Manager**.
2. Stay on the **Crosswork Summary** tab. Read the tiles for the cluster (or single VM), Platform Infrastructure, and the installed applications.
3. Open **System summary**. Confirm the VM is up and that CPU, memory, and disk are not in a warning state.
4. Open the **Crosswork Health** tab.
5. For each of the following applications, open the tile and confirm the application (and its microservices) are Healthy or Running:
    - Platform Infrastructure
    - Optimization Engine
    - Active Topology
    - Element Management Functions / Device Lifecycle Management
    - Service Health, if it is installed (chapter 8 uses it)
6. Open **Alarms** or **Events** on an application tile only far enough to see whether anything is raised against the platform itself. Device alarms belong in chapter 4 and chapter 9.

**Result:** You can name the applications this lab uses and their health state.

Leave restart, drain, and maintenance controls untouched. You already left maintenance mode in chapter 1.

## 2.2 Providers

CNC learns the routed topology from SR-PCE. CNC provisions services through NSO. Both are providers.

1. Go to **Administration > Manage Provider Access**.
2. Find the SR-PCE provider that points at the PCE CML lab (`xrv9k-25.4.1-pce-for-CNC7.2-ipv6`). Read reachability and the protocol state. You want the provider reachable, with the topology feed up.
3. Find the NSO provider. Read reachability. You want NSO reachable.
4. Open a provider only far enough to see the credential profile name. The profile is a pointer. The password stays in the profile store.

**Result:** SR-PCE and NSO are both reachable.

If SR-PCE is down, stop before chapter 3. If NSO is down, you can still walk topology and element management; wait until NSO is reachable before chapters 6 and 7.

## 2.3 Data Gateway and collection at a glance

Interface counters, inventory detail, syslog, and SNMP traps arrive through a Crosswork Data Gateway (CDG). This dCloud session may use a dedicated Data Gateway VM or the embedded collector on a single VM. Either way, devices must be attached and collection must be running.

1. Go to **Administration > Data Gateway Management**.
2. Confirm the gateway (or pool) is enrolled and administratively Up.
3. Open the gateway and confirm the XR lab devices are attached to it.
4. Open **Administration > Collection Jobs** (or **Administration > Data Gateway Management > Collection Jobs**, depending on the build).
5. On **Bulk jobs**, confirm system jobs are READY or Successful, not Failed.
6. On **Parameterized jobs**, confirm default reachability jobs exist. Policy-driven and service-based jobs appear later, after you use performance policies and Service Health.

**Result:** You can point at the collector that feeds inventory, alarms, and utilization. You will read collection jobs in more detail in chapter 9.

## 2.4 What you leave alone

Credential profiles, AAA, certificates, backup jobs, and licensing are visible from Administration and from **Device Management > Credential Profiles**. Open a profile only if a device is unreachable and you need to compare the profile name.

Backup, restore, and **Add OS patch** put the cluster into maintenance mode. Patching is a guided walk in chapter 9. Do not start it here.

## Done when

- Platform Infrastructure, Optimization Engine, Active Topology, and element management are Healthy or Running.
- The SR-PCE provider is reachable.
- The NSO provider is reachable.
- A Data Gateway (or embedded collector) is Up and the XR devices are attached.
- Collection jobs are not in a Failed state.
