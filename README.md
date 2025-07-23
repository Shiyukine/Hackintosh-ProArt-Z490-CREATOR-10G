# OpenCore Asus Proart Z490 Creator 10G 

Using macOS Sequoia

![alt text](Resources/image.png)

## Hardware
- Intel i9-10850k
- Asus Proart Z490 Creator 10G 
	- 2.5Gbit Ethernet: Intel I225-V
	- Audio: Realtek S1220A 8-Channel
    - Thunderbolt 3
- RAM: 32GB 2400Mhz DDR4
- GPU: Intel Graphics UHD 630 & RTX 2070 & GTX 1060 6GB
- Audio: Duet 2 by Apogee
- Ethernet: Hyper 10G LAN

## Working
- [x] **Audio**: Duet 2 by Apogee
- [x] **USB**: some ports USB 2.0 and USB 3.0, not USB C port
    - You need to remap
    - Remap using https://github.com/USBToolBox/tool
- [x] **2.5Gbit Ethernet (Intel I225-V)**: Hyper 10G LAN not working
- [x] **Sleep/Wake**: using iGPU only
- [x] **Shutdown (F1 problem)**
- [x] **Restart (F1 problem)**

## Not working/not tested
- [ ] Realtek S1220A 8-Channel
- [ ] RTX 2070
- [ ] GTX 1060 6GB
- [ ] Thunderbolt 3
- [ ] DRM
- [ ] Hyper 10G LAN

## Warning
- Using RELEASE opencore build:
    - Verbose is disabled
    - `TargetMisc -> Debug -> Target` is `3`
- To use DEBUG build:
    - Replace the following files with the [release versions](https://github.com/acidanthera/OpenCorePkg/releases):
        - `EFI/BOOT/BOOTx64.efi`
        - `EFI/OC/Drivers/OpenRuntime.efi`
        - `EFI/OC/OpenCore.efi`
- `SecureBootModel` set to `Disabled`. You need to disable it to complete installation to avoid bootloop ([see this](https://www.reddit.com/r/hackintosh/comments/1cdvijs/opencore_bootloader_menu_keeps_bootlooping_to/))
- You need to generate your [platform info](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#platforminfo):
    - Use Windows to get Apple ROM
    - In `SystemProductName`: modify `iMac20,1` by `iMac20,2`

## Credits
- OpenCore: https://github.com/acidanthera/OpenCorePkg
- Existing configuration Z490 (fix iGPU): https://github.com/xiaovie/Hackintosh-ROG-Z490-series-motherboard-OpenCore