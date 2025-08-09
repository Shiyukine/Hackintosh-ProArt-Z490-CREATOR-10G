# OpenCore Asus Proart Z490 Creator 10G 

Using macOS Sequoia 15.6, OpenCore 1.0.4

![alt text](Resources/image.png)

## Hardware
- CPU: Intel i9-10850k
- Motherboard: ASUS ProArt Z490-CREATOR 10G
	- 2.5Gbit Ethernet: Intel I225-V
	- Audio: Realtek S1220A 8-Channel (ALC1220)
    - Thunderbolt 3
- RAM: 32GB 2400Mhz DDR4
- GPU: Intel Graphics UHD 630 & RTX 2070 & GTX 1060 6GB
- Audio: Duet 2 by Apogee
- Ethernet: Marvell 10G LAN (Aquantia AQtion AQC107)
- Storage: Crucial P3 Plus 4.0 NVMe PCIe M.2, Crucial MX300 SATA SSD, Crucial Mx100 512GB 2.5" SSD, Crucial Bx200 960GB 2.5" SSD
- Bluetooth: TP-Link UB400

## Working
- [x] **Audio**: 
    - Duet 2 by Apogee with/without driver
    - Realtek S1220A 8-Channel
- [x] **USB**: mapped some ports USB 2.0 and USB 3.0, not USB C port
    - You will need to remap
    - Remap using https://github.com/USBToolBox/tool
- [x] **Ethernet**: 
    - 2.5Gbit Ethernet (Intel I225-V)
    - 10Gbit Ethernet (Aquantia AQtion AQC107)
- [x] **Sleep/Wake**: using iGPU only
- [x] **Shutdown/Restart**: fixed BIOS reset or sent into Safemode after reboot/shutdown
- [x] **Bluetooth**: USB Bluetooth dongle TP-Link UB400
- [x] **Updates**: From Sequoia 15.5 to Sequoia 15.6
- [x] **Apple Secure Boot**: ApECID is generated

## Not working/not tested
- [ ] RTX 2070
- [ ] GTX 1060 6GB
- [ ] Thunderbolt
- [ ] DRM

## Warning
- Using RELEASE opencore build:
    - Verbose is disabled
    - `TargetMisc -> Debug -> Target` is `3`
- To use DEBUG build:
    - Replace the following files with the [release versions](https://github.com/acidanthera/OpenCorePkg/releases):
        - `EFI/BOOT/BOOTx64.efi`
        - `EFI/OC/Drivers/OpenRuntime.efi`
        - `EFI/OC/OpenCore.efi`
- You need to set `SecureBootModel` to `Disabled` to update or to complete installation to avoid bootloop ([see this](https://www.reddit.com/r/hackintosh/comments/1cdvijs/opencore_bootloader_menu_keeps_bootlooping_to/))
- You need to generate your [platform info](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#platforminfo):
    - Use Windows to get Apple ROM
    - In `SystemProductName`: modify `iMac20,1` by `iMac20,2`
- Working with HDMI only
- `IntelBluetoothFirmware.kext` is modified to work with some USB dongle ([see this](https://www.reddit.com/r/hackintosh/comments/16w2elb/how_to_make_generic_usb_bluetooth_50_csr_dongle/)):
    - You need to get your Vendor ID and Product ID of your Bluetooth dongle if it is not 0x0A12 and 0x0001 respectively and modify `IntelBluetoothFirmware.kext/Contents/Info.plist`
- `VT-d` must be enabled in BIOS, otherwise 10Gbit Ethernet (Aquantia AQtion AQC107) will not work 
- If you have a crash (kernel panic) a few seconds or minutes after booting, modify `Primary Display` to `CPU Graphics`
- If you have `Failed to prepare the software update` error:
    - Verify that `SecureBootModel` is set to `Disabled`
    - Reset NVRAM
    - Use Terminal:
        ```bash
        sudo softwareupdate -i -a -R
        ```

## Dual boot with different disks
- If you already have EFI partition:
    - Mount the macOS disk EFI partition
    - Copy `EFI` folder to the macOS disk EFI partition 
- If you don't have EFI partition without wiping (using macOS __installer__):
    - Type `diskutil list` to find the macOS disk number
    - Resize the APFS partition to create a new partition for EFI:
        ```bash
        diskutil apfs resizeContainer diskXsY 274.6GB
        ```
    - Force unmount the disk:
        ```bash
        diskutil unmountDisk force /dev/diskX
        ```
    - Create a new partition for EFI:
        ```bash
        gpt add -i [new partition number] -b $(gpt show diskX | grep free | awk '{print $1}') -s $(echo "200*1024*1024/512" | bc) -t C12A7328-F81F-11D2-BA4B-00A0C93EC93B /dev/diskX
        ```
    - Copy `EFI` folder to the macOS disk EFI partition (using macOS)

## Other drivers
Available in `./Resources/Drivers`:
- `duet-2.5c.zip`: for Duet 2 by Apogee

## Sources
- OpenCore Install Guide: https://dortania.github.io/OpenCore-Install-Guide
- OpenCore: https://github.com/acidanthera/OpenCorePkg
- Existing configuration Z490 (fix iGPU): https://github.com/xiaovie/Hackintosh-ROG-Z490-series-motherboard-OpenCore
- Bluetooth dongle fix: https://www.reddit.com/r/hackintosh/comments/16w2elb/how_to_make_generic_usb_bluetooth_50_csr_dongle/
- Fix for bootloop in installer: https://www.reddit.com/r/hackintosh/comments/1cdvijs/opencore_bootloader_menu_keeps_bootlooping_to/
- USB remap: https://github.com/USBToolBox/tool