# What is the OCDT Partition?
In the OPLUS ecosystem (encompassing OnePlus, OPPO, and realme), OCDT stands for Oppo Component Details Table (or Oppo Custom Device Table). It is a **proprietary, non-standard system partition** used by the unified OPLUS vendor framework *to store factory-programmed, hardware-identifying metadata specific to a given device variant*.

**Key Contents of the OCDT Partition**
Project ID / Board Identifier (`prjName` / `prjNum`): Defines the target mainboard model (e.g., `Project ID 21615`, `22811`, etc.). The primary bootloader (`abl`) checks this ID to match Device Tree Blobs (`DTB`) and hardware drivers during early boot phases.
***Region & Carrier Locks:*** Stores regional flags (IN, EU, NA, CN) used for regional lock mechanisms and feature restrictions across geographic software builds.
***Hardware Revision & Calibration Data:*** Contains parameters indicating specific hardware revisions, display panel batches, camera sensor variants, and modem configurations.
***DRM & Device Attestation Data:*** Interacts with the TrustZone/TEE and Keymaster components to validate system integrity.

# Why is OCDT So Important?
***Boot Validation:*** During the boot sequence, **the primary bootloader reads `OCDT`** to verify that *the loaded firmware matches the precise hardware revision*.
***Cross-Flashing Protection:*** Mismatches between the `OCDT` partition and flashed firmware (*such as cross-flashing OxygenOS onto a `CN`-spec device*) will cause the bootloader **to reject the boot image**.
***Custom ROM Safety:*** Corrupting or improperly swapping `OCDT` can break **touch drivers**, **fingerprint sensors (FOD)**, **disable Fastboot access completely**, or **hard-brick the device**.

## OnePlus Nord CE4 Lite case
Indian device version is suffering from OCDT corruption and lacks Fastboot mode access completely.
EDL flash can fix it but there's a catch... ARB makes EDL flash a 10 times harder procedure, requiring developing a new bypass to enter firehose mode.

| CPH2619export_16.0.1.301EX01 or lower | CPH2619export_16.0.1.303EX01 and greater |
|:--------------------------------------|:-----------------------------------------|
| You are able to flash hybrid image through EDL | You aren't able to flash hybrid image since there's no new bypass for flashing against ARB |
| ARB index `0x0` | ARB index `0x1` | 

For your further researches my dear friend, I took ocdtEUcamry.img from my alive EU device and took ocdtINcamry.img from easy JTAG dump from CPH2619export_16.0.5.700EX01 (don't worry these ocdt aren't affected by OTA updates).

I am not able of resolving such a big problem.

Even more is that I'm doing this all alone just for someone else in the possible future to take my place in AFK camry community.
