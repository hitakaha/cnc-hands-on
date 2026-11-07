# 1. Preparation

**Time:** 50 minutes

**Objective:** Access dCloud environment, leave maintenance mode, start the CML lab, and log in to a controller that is ready for work.

!!! note "Instructor"
    Maintenance mode shuts the applications down gracefully, and bringing them back takes several minutes.

## 1.1 Lab setup and CNC
The lab setup
In this scenario, following is the lab setup.

- CNC version 7.2.0 Single VM
- P and PE routers: C8000 on CML
- PCE: XRv9k

For P and PE routers, Cisco 8000 hardware emulator on CML is used.
For more information about Cisco 8000 hardware emulator, please refer to following:

[https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000-emulator/cisco8000-hardware-emulator-installation-guide.pdf](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000-emulator/cisco8000-hardware-emulator-installation-guide.pdf).

XRv9 for PCE is running in a difference CML instance, connected by multiple external connectors.

The difference between Single VM (SVM) and cluster
There are two types of CNC deployment: Single VM (SVM) and cluster.
In Single VM, all nodes, NSO and Crosswork Data Gateway (CDG) are in single VM.
In cluster deployment, each component must be set up as a separate VM.
![dcloud](img/01/01-5-svm-cluster.png)


## 1.2 Disable maintenance mode

Use maintenance mode to temporarily suspend Crosswork Network Controller operations for maintenance or restart activities and resume normal service.
Maintenance mode provides a graceful shutdown for system updates and synchronizes application data before suspending services.

For more information about the maintenance mode, please refer to following:

[https://www.cisco.com/c/en/us/td/docs/cloud-systems-management/crosswork-infrastructure/7-2/AdminGuide/b-cisco-crosswork-admin-guide-7-2/g-vm-management/t-manage-maintenance-mode-settings.html](https://www.cisco.com/c/en/us/td/docs/cloud-systems-management/crosswork-infrastructure/7-2/AdminGuide/b-cisco-crosswork-admin-guide-7-2/g-vm-management/t-manage-maintenance-mode-settings.html)

Follow the steps blow to confirm the maintenance mode is off. If not, disable the maintenance mode.

1. Sign in to the dCloud and click "View" button (Note, the scenario name could be different).
![dcloud](img/01/01-dcloud.png)
2. Access Ubuntu2204-1T-Desktop (use VM Consoel), run Firefox on the Desktop.
![dcloud](img/01/01-2-ubuntu.png)
Click "CNC" in the bookmark and access CNC (https://[2001:db8:134::200]:30603).
![dcloud](img/01/01-3-cnc.png)
After the log in, dashboard is displayed.
![dcloud](img/01/01-4-logged-in.png)
3. Go to **Administration > Settings > System Settings > Maintenance Mode**.
4. If a popup already offers to turn maintenance mode off, accept it.
5. If the Maintenance slider is On, drag it to Off.
![dcloud](img/01/02-turn-off-maintenance-mode.png)

6. Wait. The applications need several minutes.

**Result:** The maintenance slider stays Off, the UI loads the normal home page.


## 1.3 Confirm CML labs are running
In this lab, two CML instances are connected via multiple external connecors.
Rivew the network topology and familiarize with the connection.

![dcloud](img/01/03-2-two-cmls.png)

Follow the steps below and confirm both CMLs are up and running.

1. Open another tab on Firefox and click "CML2 for XR" in the bookmark.
2. Confirm "5-nodes_L3VPN 8201-32FH 25.3.1-ipv6" scenario is running. If not, start the scenario.
![dcloud](img/01/03-run-xr.png)
3. Open another tab on Firefox and click "CML2 for PCE" in the bookmark.
4. Confirm "xrv9k-25.4.1-pce-for-CNC7.2-ipv6" scenario is running. IF not, start the scenario.
![dcloud](img/01/04-run-pce.png)


**Result:** The lab is booted. CNC can reach the devices after the providers and the Data Gateway are healthy. You confirm that reachability in the next chapter.


## 1.4 Sync-from devices

A key component of CNC is Network Services Orchestrator (NSO).
NSO holds devices configuration to an internal database called CDB.
The device configuration and CDB must be in sync for CNC to execute the provisioning.

![dcloud](img/01/07-2-nso-architecture.png)

If there is a diff, the state is changed to "out-of-sync".
Once that happens, execute "sync-from" in **Device Management** and sync the configuration beween CNC and the devices.

Follow the steops below and get familialize with the sync operation.

1. Access CNC and go to **Device Management > Network Devices**
2. Select all devices and choose **Sync-from devices to NSO** from Actions
![dcloud](img/01/05-device-to-sync.png)
3. Go to **Inventory Jobs** and wait for the task to be completed
![dcloud](img/01/06-inventory-jobs.png)
4. Open NSO by clicking "NSO" button located in the top right.
![dcloud](img/01/07-nso.png)
5. Just click "SSO Login" to login to NSO, no need to fill out username and password.
![dcloud](img/01/08-login-sso.png)



**Result:** All devices are synec to CNC. 
You are in the UI as yourself, and the controller is in normal operating mode.

## Done when

- Maintenance mode is Off.
- Log in to CNC
- The CML labs are booted.
- All devices are synced
