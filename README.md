# AMD_OC_Ryzen

This commit provides the basic contents for an EFI folder to successfully boot an ASRock X570 Creator motherboard,
using a Ryzen 9 CPU such as a 3950X. The contents work with either Mojave or Catalina. The intended SMBIOS is iMacPro1,1,
although provisions are available for running MacPro7,1, which will be described below.

There are notable dependencies on OpenCore, the suggested bootloader. OpenCore is best updated via Pavo's OCBuilder app
(https://github.com/Pavo-IM/ocbuilder/releases). Accordingly, specific files compiled during an OC build will not be 
necessarily updated in this repository, but will be temporarily present to fascilitate creation of an EFI folder (v056 as of 3/1/2020).

This repository will attempt to keep an up-to-date basics of an EFI that will work on an established computer. For details regarding how to setup OpenCore, how to create a bootable installation, how to trouble shoot errors, how to optimize your sysstem and so forth, please see the AMD-OSX/AMD-Vanilla repository at https://github.com/AMD-OSX/AMD_Vanilla/tree/master.

This repository will maintain the ACPI, Driver and Kexts folders along with the config.plist and 
config-Only-For-Storage.plist files. These need to be placed into an EFI folder which is located on the EFI partition
of the boot drive (see structure below).

The Driver and Kext folders can be updated outside this commmit by running OCBuilder and transferring the appropriate, newly udpated files to their respective folders. If updated, please study the Docs to see if the structure of the config.plist file has changed. OpenCore is evolving and consequently new versions can substantially effect the overall structure and functioning of the config.plist file.
    
## Contents

### ACPI



Presently, TB3 while working, is still incomplete (the TB deice must be connected before boot and there is no hot-plug capability). Check discusson sites listed below. Hopefully, the only update required to make TB3 fully functional will be an more complete SSDT-TB file than the one presently being used.

### Kexts
The contents of the Kexts folder can be broken down into various groups. First are the AGPMInjector kexts (created by
Pavo's AGPMInjector app (https://github.com/Pavo-IM/AGPMInjector/releases). Here is provided several variations to save 
time based on commonly used GPUs. These should be paired with similarly named files within the ACPI folder and each should
be enabled within the config.plist file as required. Example: selected AGPMInjector-iMacPro1,1-RX580.kext and SSDT-X570-RX580-slot-1.aml as pairs, ensuring both are entered in the proper sections of the config.plist file and that both are enabled.

Other groupings within the Kexts folder include the BT/Wifi kexts: AirportBrcmFixup, BrcmBluetoothInjector, BrcmFirmwareData, BrcmPatchRAM3, and BT4LEContinuityFixup. If you've swapped out the stock Intel BT module for a Mac-compatible version (as described here https://forum.amd-osx.com/viewtopic.php?p=53060#p53060), you'll want all of these enabled within the config.plist file. If you've added a PCIe BT/WiFi card, then most of these are optional. Other files will vary whether you're using a swapped BT (SBT) or PCIe BT (PCIeBT). These changes will be described below.

The other grouping are the essential kexts: AppleALC, AppleMCEReporterDisabler, Lilu, SmallTreeIntel82576_mod, VirtualSMC and WhateverGreen (WEG). MacProMemoryNotificationDisabler is required for SMBIOS MacPro7,1. Within the config.plist file, in the Kernel section, Lilu must be first in order, followed by VirtualSMC. WEG should be present before other graphics related kext files.




### Drivers



## Usage
 
- To build OpenCore using Pavo's OCBuilder. It is recommended to use the Release version.
- Move included folders for ACPI, Drivers, Kexts and the plist files into EFI/OC folder created by OCBuilder
- Final EFI folder should have a structure as shown below (OC v056 as of 3/1/2020).

## EFI Folder

       EFI----Boot----Bootx64.efi
        |
        |_____OC_____ACPI
               |       |____various *.aml files
               |
               |_____config.plist
               |
               |_____config-Only-For-Storage.plist
               |
               |_____Docs
               |       |_____AcipSamples
               |       |          |_______various SSDT files
               |       |
               |       |_____Changelog.md, Configuration.pdf, Differences.pdf, Sample.plist, SampleFull.plist
               |
               |_____Drivers
               |       |______ApfsDriverLoader.efi, AudioDxe.efi, FwRuntimeServices.efi, HFSPlus.efi
               |
               |_____Kexts
               |       |______various *.kext files
               |
               |_____OpenCore.efi
               |
               |_____Resources
               |       |_____Audio
               |                |____ various WAV files
               |_____Tools
               |       |______BootKicker.efi, CleanNvram.efi, GopStop.efi, HdaCodecDump.efi, Shell.efi, VerifyMsE2.efi
               |
               |_____Utilities
                       |_____BootInstall
                       |
                       |_____CreateVault
                       |
                       |_____LogoutHook
                      
                          
    

## Discussion

- [forum.amd-osx](https://forum.amd-osx.com/viewtopic.php?f=35&t=9645) especially for TB3 updates


## Credits

- [AlGrey](https://github.com/AlGreyy) for the idea and creating the patches
- [Andrey1970AppleLife](https://github.com/Andrey1970AppleLife)
- [AMD OS X](https://github.com/AMD-OSX/AMD_Vanilla/tree/master)
- [Download-Fritz](https://github.com/Download-Fritz) for OpenCore
- [Pavo](https://github.com/Pavo-IM) for OCBuilder and AGPMInjector
- [trulyspinach](https://github.com/trulyspinach/SMCAMDProcessor) for CPU Temp/Freq monitoring
- [vit9696](https://github.com/vit9696) for OpenCore and many of the kexts we use
