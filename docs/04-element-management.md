# 4. Element management (EMF)

**Time:** 60 minutes.

**Objective:** Use the CNC 7.1 element-management functions to read inventory, archived configuration, a configuration template, alarms, events, and performance. Deploy nothing from this chapter.

Element Management Functions in CNC 7.1 cover device onboarding, inventory, faults, metrics, software images, configuration backup, and configuration templates. ZTP is part of the same lifecycle guide. You will tour ZTP in chapter 8 so this hour stays on "what is on the device today."

## 4.1 Network devices and inventory

1. Go to **Device Management > Network Devices**.
2. Find the headend. Read operational state, reachability, and the credential profile name.
3. Confirm a Data Gateway is associated with the device. Chapter 2 introduced that mapping. This is where a missing mapping shows up as a device you cannot refresh.
4. Go to **Device Management > Network Inventory**.
5. Filter to the headend hostname or product family.
6. Open the chassis, one route processor or line card, and one pluggable if the inventory has them.
7. Open the alarm summary on that inventory row. You will come back to the full alarm list in section 4.4.

**Result:** You can walk from a hostname to a chassis and a module, and you know whether the device is reachable.

A device that stays unreachable for the whole session needs the checkpoint page: credentials, routing to the management address, and CML node state. Give it a few minutes after the lab boots before you call it failed.

## 4.2 Configuration backup

CNC can archive device configuration, compare two archives, and restore. This lab uses the archive as a reader.

1. Go to **Device Management > Configuration Backup and Restore**.
2. Find the headend.
3. Open the latest archived configuration and identify the hostname and the segment-routing or BGP section the instructor points out.
4. If two archives exist, open the compare view and name one difference, or state that the two archives match.
5. Leave **Schedule backup** and **Restore** unused.

**Result:** You have seen a real archived configuration for the headend and you know where a later change would show up.

Automatic backup on configuration change, when it is enabled under **Device Management > Configuration Preferences**, is what makes this archive useful after chapter 6. Read the preference. Leave the values as you found them.

## 4.3 Configuration templates

Device templates under Device Management are parameterized CLI (Apache Velocity template language). They are for repeating underlay or day-1 configuration across similar devices. They are a different store from the NSO L2VPN and L3VPN function-pack templates that chapter 6 uses.

1. Go to **Device Management > Configuration Templates**.
2. Open one existing template. Read the device types it targets and one variable it expects.
3. Open **Jobs** only if a previous job is listed, and read its status.
4. Close the template. Leave **Create Template** and every deploy action unused.

**Result:** You can explain the difference between this template library and the service template NSO applies when you provision an L3VPN.

Deploying a template from this screen writes CLI to live routers. That write is out of scope for a shared class.

## 4.4 Alarms and events

1. Go to **Alerts > Alarms and Events**.
2. Stay on **Alarms**. Use the category list to see system, network, and device alarms as separate sets.
3. Filter to the headend if the filter allows it.
4. If an alarm is present, record severity, source, and whether it is acknowledged. Leave acknowledge, clear, and annotate unused unless the instructor gives you a lab alarm of your own.
5. Switch **Show** from Alarms to **Events**.
6. Read the newest event. An event is a moment in time. An alarm remains until it clears. Use one sentence to put the event you found into one of those two buckets.

**Result:** You can open both views and you did not clear someone else's alarm.

An empty alarm list is a valid result on a healthy CML lab. Write "no active device alarms" and move on.

## 4.5 Performance

Link colors on the topology map come from interface utilization collected for Optimization Engine. Element management adds polling policies and device-level metrics.

1. Go to **Device Management > Performance Policies**, or the performance page under Device Management on your build.
2. Open one existing policy. Read which devices or groups it covers, how often it polls, and which threshold raises an alarm.
3. Return to the headend in **Network Inventory** or **Network Devices** and open the metrics or interface view. Compare one interface counter with the utilization you wrote down in chapter 3.
4. Leave the policy inactive/active switch as you found it. A new polling policy on every seat multiplies SNMP load on the lab.

**Result:** You can say where a threshold is defined, and you can point at one live counter.

## Done when

- The headend is in Network Devices and in Network Inventory.
- You opened an archived configuration and a device template, and you deployed neither.
- You opened Alarms and Events.
- You can point at the polling policy that would raise a threshold alarm.
