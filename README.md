# Samsung NC10 OpenCore EFI
EFI and related hackintosh resources to run Mac OS X 10.6.3 Snow Leopard on your Samsung NC10 via OpenCore

### TL;DR guide
**Preinstall**
1) Get a Mac OS X Snow Leopard 10.6.3 install image (*only* 10.6.3 will work with these files: not 10.6.0, nor 10.6.7, nor any other version!) and restore it to an USB drive using your preferred method
2) Place the EFI folder on your USB drive's EFI system partition, then install OpenDuet on the same drive. If unsure how, follow Dortania's [Legacy Install Setup guide (found at the end of the diskpart method) for Windows](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html#diskpart-method) or [the Legacy Setup guide for macOS](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#legacy-setup)
3) Generate your MacBook1,1 SMBIOS info and add it to EFI/OC/config.plist *(optional: you may also want add the -v flag to boot-args and show the boot picker)*
4) **IMPORTANT:** Replace mach_kernel in the USB installer's HFS+ partition with the one found in this repo. **If you skip this step, the installer will NOT be able to boot.**
5) On the Samsung NC10, get in the BIOS settings and make sure *EDB (Execute Disable Bit)* in the Advanced tab is set to *Enabled*
6) You're now ready to boot the USB installer. Install Snow Leopard as usual and, once done, shut down the netbook. *Don't attempt to boot into Mac OS X yet as that won't work, follow the postinstall steps below instead*

**Postinstall**
1) The installer won't copy automatically our custom mach_kernel to the OS X partition on the netbook's internal drive, so you must do it manually before you're able to boot. **Once again, you will NOT be able to boot if you skip this step.**
2) Copy & extract GMA 950 Kexts.zip, install the PKGs *(natitintel.pkg shouldn't be needed and is only kept for reference)*, then [add the GMA 950 device properties for Laptops in your config.plist](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/legacy-intel/#property-injection) with the exception of device-id, as it shouldn't be needed with these modded Kexts. Once done, hardware graphics acceleration should be working
3) Copy & install VoodooHDA-0.2.62.pkg. Once done, audio input & output should be working *(if you get issues with microphone feedback, you can work around the issue by going to System Settings - VoodooHDA and changing the mic input line volumes)*
4) The stock Wi-Fi card should be incompatible with Snow Leopard (iirc) and should be replaced with a compatible one if you want working Wi-Fi. I've replaced mine with a RT2700E and included its driver install pakcage (STA_RT2860D-1.2.2.0UI-3.0.0.0_2010_05_18.dmg) as it seems to be somewhat hard to find on the net, but unless you replace yours with the same exact chipset as mine, you should instead follow whichever instructions are for the specific chipset you choose

### FAQ
**- Why does this only work with Mac OS X 10.6.3?**
Getting Mac OS X 10.6.2 or later to boot on an Intel Atom CPU requires a custom Mach kernel, as this processor family has never been used by real Macs and Apple added a CPU check preventing OS X's kernel from booting on an Atom starting from said version. This means you *don't* need a custom kernel for 10.6.1 or earlier, but you may encounter issues not addressed by this EFI or guide. At the same time, every Snow Leopard version comes with a slightly different Mach kernel, meaning they are *not* interchangeable between releases: a custom Mach kernel for 10.6.3 *won't* work on 10.6.8, for example. Still, custom kernels for Atom CPUs were made for every Snow Leopard release and they can be found online with relative ease, but they may require extra work to get running on the NC10 (and, reportedly, some are more stable than others). In a nutshell, what I've provided in this repo is a stable, fully working configuration with no major issues - other versions may work, but *here be dragons*.


**- Are all the SSDTs included in this EFI really needed?**
Likely not. SSDT-EC, SSDT-PNLF and SSDT-XOSI are definitely required for a variety of reasons, but you might still be able to boot without the others. However, all SSDTs found here have been generated specifically for the NC10 using SSDTTime and the addition of SSDT-HPET, SSDT-PMC and SSDT-USBX doesn't seem to have any negative consequences, so they're still included both for reference and just in case some specific configuration/model variation needs them.

*(FAQ is still WIP)*
