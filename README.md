
OpenEmbedded/Yocto Digi Embedded Dual-SDMMC Layer
=====================================

This layer enables dual sdmmc support on the Digi ConnectCore MP13 SOM (wired only variant).

This implementation was inspired by a customer who wanted to add an eMMC flash to sdmmc1 and a microSD card to sdmmc2 (the default is microSD on sdmmc1).

SUPPORTED PLATFORMS
------------------
This layers was created specifically for the Digi ConnectCore MP13 (CCMP13) system on module. It should, however, work on the CCMP157 SOM.
* [CC-ST-DX58-ZK](https://www.digi.com/products/models/cc-st-dx58-zk)

SUPPORTED DEY RELEASES
--------------

* DEY-4.0 (Kirkstone)

  
INSTALLATION
----------
1. Install Digi Embedded Yocto (DEY), per the published [instructions](https://www.digi.com/resources/documentation/digidocs/embedded/dey/4.0/ccmp13/yocto-install-dey_t.html).
2. Clone meta-dual-sdmmc layer to your local workstation.
   #> cd <DEY_INSTALLDIR>/sources
   #> git clone https://github.com/chaegle/meta-dual-sdmmc.git

CREATE AND BUILD A PROJECT
----------------------
3. Create a project for your supported platform.
4. Add the meta-dual-sdmmc layer to your project's bblayers.conf file.

	    user@computer: user/myproject$ bitbake-layers add-layer <path-to-meta-dual-sdmmc>

6. OPTIONAL - include e2fsprogs in to the rootfs image, in order to be able to create ext2/3/4 file systems. Edit and add below line to the project's local.conf file.

   IMAGE_INSTALL:append = " e2fsprogs"

7. Build the image.

	    #> bitbake core-image-base

8. Deploy artifacts to the target.
9. Boot and confirm that U-boot now recognizes both MMC interfaces.

From U-Boot you should see a line that reads:  

	    MMC:   STM32 SD/MMC: 1, STM32 SD/MMC 2
  
The command 'mmc list' should report two interfaces.

	    =>mmc list
	    STM32 SD/MMC 1
	    STM32 SD/MMC 2
  
10. From Linux one should see two mmcblk devices.

	    user@computer: ~# ls /dev/mmc*
	    /dev/mmcblk0		/dev/mmcblk1

11. Use fdisk to create the desired partition(s) on the device(s).

LIMITATIONS
-----------
The modifications in this layer are based on a specific customer design, where an eMMC was connected to sdmmc1 and the microSD card to sdmmc2.

The default Digi provided reference design has the microSD card connected to sdmmc1. Interface sdmmc2 is available for customer use. On the wireless variant of the CCMP13 P/N: [CC-WST-DX58-NK](https://www.digi.com/products/models/cc-wst-dx58-nk) sdmmc2 is internally connected to the on-board wireless radio, thus not available for external use.

Additionally, it seems as though there is an issue using a bus-width of 8, on the eMMC. In both U-Boot and Linux the eMMC will still work, however, they will report being in 4-bit mode.
 

