# 8. Lifecycle management and ZTP

**Time:** 20 minutes.

**Objective:** Walk the Zero Touch Provisioning screens so you can recognize images, configuration files, serial numbers, profiles, and devices. Leave every router in its current boot state.

CNC 7.1 puts ZTP under element management: **Device Management > Zero Touch Provisioning**. Classic ZTP uses DHCP and HTTP so a new router can pull an image and a configuration. Secure ZTP (RFC 8572) adds ownership vouchers and a signed exchange. This controller may show both. The PTA lab routers are already onboarded through the normal device inventory. They are the wrong devices to reload into ZTP during class.

## 8.1 Tour

Work down this list. Open each page, read the columns, and close it.

1. **Device Management > Software Management**. This is the image and SMU store. Read one image name and the platform family, if any file is present. Upload nothing.
2. **Device Management > Zero Touch Provisioning > Configuration files**. These are ZTP day-0 scripts, with replaceable parameters. They are a third store, different from Device Management templates and from NSO service templates. Open one file if it exists and find a single placeholder. Download the sample XR script only if the instructor wants the room to see the comments. Upload nothing.
3. **Device Management > Zero Touch Provisioning > Serial Numbers & Ownership**. A device enters ZTP when its serial number is known here. Read the columns. Add no serial and import no CSV.
4. **Device Management > Credential Profiles**. ZTP devices need a profile too. You already know this page from chapter 2. Confirm the lab profile is still the one you saw. Edit nothing.
5. **Device Management > Zero Touch Provisioning > ZTP profiles**. A profile binds an image, a configuration file, and a device family. Open one profile tile if the lab has one. Create none.
6. **Device Management > Zero Touch Provisioning > Devices**. This list is ZTP state, which is separate from **Network Devices**. If it is empty, the lab simply has no ZTP job in progress. That is the expected classroom state.

**Result:** You can name the six pages and say which object each one holds.

## 8.2 The workflow you are not running

Say this sequence back to the instructor, from the screens you just opened:

1. The image is in Software Management.
2. The day-0 configuration file is in the ZTP configuration store.
3. The serial number is loaded, with an ownership voucher when the flow is Secure ZTP.
4. A credential profile exists.
5. A ZTP profile ties the image and the configuration to a device family.
6. A device row tracks state from boot to onboarded.
7. DHCP, or the secure ZTP bootstrap, points the hardware at this controller. After onboarding, the device also appears under **Device Management > Network Devices**.

The home page dashboard can show a ZTP status tile while a job runs. You may look for the tile. An absent tile means no active job.

## 8.3 Why this stays a tour

A real ZTP run reloads a device, depends on DHCP options or a secure bootstrap, and consumes a serial number. CML guests in this topology are already part of the IGP. Reloading one of them removes it from the map for every seat. Image upload and serial import are the same class of shared change.

Software image management for an already-managed router (upgrade and SMU) lives next to ZTP under **Device Management > Software Management**. Recognize the page. Start no upgrade.

## Done when

- You visited Software Management, ZTP configuration files, serial numbers, ZTP profiles, and ZTP devices.
- You can separate those objects from a Device Management template and from an NSO service template.
- No image, serial, profile, or device row was created by your seat.
