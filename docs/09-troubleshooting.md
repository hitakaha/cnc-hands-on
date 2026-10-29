# 9. Troubleshooting

**Time:** 45 minutes.

**Objective:** Collect CNC showtech, read collection jobs, read alarms, and walk the add-patch workflow. Applying a patch is an instructor decision because CNC puts itself into maintenance mode to install it.

This chapter is about the controller, not about rewriting router configuration.

## 9.1 Alarms

1. Go to **Alerts > Alarms and Events**.
2. Filter **Alarms** by category: system, network, and device.
3. For each category, record whether anything is raised. System alarms belong to CNC itself (disk, application, collector). Device alarms belong to the XR nodes.
4. Open one alarm if any exist. Record severity, source, time, and description. Leave acknowledge and clear unused unless the instructor assigns that alarm to you.
5. Switch to **Events** and find the event that created the alarm, if one exists.

**Result:** You know whether the platform, the network, or a device is currently unhappy.

Use alarms together with chapter 2 health tiles. A Healthy application tile and a system alarm can both be true; the alarm is the detail.

## 9.2 Collection jobs

Empty inventory, blank utilization, and Service Health stuck on Initiated are usually collection problems, not VPN problems.

1. Go to **Administration > Collection Jobs**, or **Administration > Data Gateway Management > Collection Jobs**.
2. Open **Bulk jobs**. These are created by the system, the UI, or the API. Record Status (READY, NOTREADY, FAILED) and collector type (SNMP, CLI, gNMI, syslog, trap).
3. Open a Successful or READY job. Read the device list and the sensor path or CLI command.
4. Open **Parameterized jobs**:
    - **Default jobs** — reachability
    - **Policy-driven jobs** — Performance Policies from chapter 4
    - **Service-based jobs** — created when you started Service Health on the VPWS
5. Find a service-based job related to `pta-vpws-01` or to L2VPN monitoring. Confirm it is not Failed.
6. If a job is Failed or Degraded, open the job details and read the error. Common causes:
    - Device not attached to a Data Gateway
    - SNMP or CLI credentials wrong
    - Device unreachable (chapter 3 CLI will already have shown this)
    - Destination in Error, while collection still runs and increments an error count

Do not create a new bulk job in class. Do not delete a system job.

**Result:** You can point at the job that feeds Service Health, and you know whether it is READY.

## 9.3 Showtech

Showtech is what you collect before you open a TAC case. CNC 7.2 collects cluster logs and metrics from **Crosswork Manager**. Application showtech is per application. Data Gateway showtech is a separate encrypted download.

### Cluster and application

1. Go to **Administration > Crosswork Manager**.
2. Open **Crosswork Summary > System summary**.
3. Click **Actions** and note the showtech options:
    - **Request all** — logs and metrics
    - **Request metrics**
    - **Collect logs**
4. If the instructor wants a file from this VM, collect **logs** (not Request all) so the bundle stays smaller. Otherwise, only open **Actions > View showtech jobs** and read any previous job.
5. To collect for one application, return to **Crosswork Health**, open the application tile (for example Service Health or Platform Infrastructure), and use that tile's showtech action. Application logs are collected separately from the cluster bundle.
6. In **View showtech jobs**, read status. Publish or download only if the instructor will take the file. Delete nothing.

**Result:** You know where cluster showtech and application showtech are started, and you did not fill the disk unless the instructor asked for a collect.

### Data Gateway (read only unless asked)

1. Go to **Administration > Data Gateway Management**.
2. Select the gateway.
3. Open **Actions** and find **Download Showtech**.
4. Do not download unless the instructor asks. The file is encrypted. The instructor keeps the passphrase and decrypts the file if TAC needs it. Do not put a passphrase in chat or in this guide.

**Result:** You can show where CDG showtech lives.

## 9.4 Add patch

CNC 7.2 installs OS security patches (SPACK) from **System management**. Application bundles are added from **Application Management**. Both are maintenance operations. Adding an OS patch automatically turns maintenance mode on, applies the bundle node by node, and turns maintenance mode off. Rollback is not supported. A small lab VM still takes a long time.

!!! note "Instructor"
    Walk the screens with the room. Apply a patch only if a lab SPACK or application file is staged and you have time for maintenance to bounce. Students do not click **Add** unless you say so.

### OS patch screens

1. Go to **Administration > Crosswork Manager**.
2. Open the **System management** tab.
3. Find **Add OS patch**. Open the dialog far enough to see that the source is URL or SCP. Do not enter a server password into the guide or into a screenshot.
4. Close the dialog unless the instructor is applying a staged patch.
5. Open **System updates** (or the list of already installed patches). Record whether this dCloud image already shows 7.2.1-SP or another installed bundle.
6. Open **Job History** on System management. This is where add-to-repository and per-VM apply jobs appear after someone adds a patch. Read any existing job. Do not retry a job unless the instructor is recovering a failed apply.
7. After a successful apply (instructor only), verification is **Administration > Crosswork Manager > System summary > View details > Package details**.

If the instructor does apply a patch, wait. Do not log in while maintenance mode is on. When the UI returns, repeat the health checks from chapter 2.

### Application file

1. Go to **Administration > Crosswork Manager > Application Management**.
2. Note **Add new file**. That is how an application CAPP or a patch bundle is uploaded.
3. Do not upload a file unless the instructor provides one and asks you to.

**Result:** You can find Add OS patch, System updates, Job History, and Application Management. You did not start a patch on your own.

## 9.5 If something from an earlier chapter is still broken

| Symptom | First check |
| --- | --- |
| Empty topology | SR-PCE provider, BGP-LS, `show pce ipv6 topology summary` |
| Device unreachable in Network Devices | CML node booted, management IPv6, credential profile |
| Thin inventory / no utilization | Data Gateway attachment, collection job status |
| SR policy preview empty | SRv6 tab, `show segment-routing srv6 locator`, PCEP peer |
| VPN out of sync | NSO provider, Check sync, then chapter 7 optional sync-from only if asked |
| Service Health stuck on Initiated | Parameterized service-based collection job |

## Done when

- You opened Alarms and Events and recorded what is (or is not) raised.
- You found bulk and parameterized collection jobs, including a service-based job for the VPWS when monitoring is on.
- You can start cluster showtech from System summary and you know that application and CDG showtech are separate.
- You walked Add OS patch and Job History without starting an unplanned maintenance window.
