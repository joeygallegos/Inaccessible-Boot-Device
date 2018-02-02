# Inaccessible-Boot-Device
Boot your computer using a USB bootable media.

### Refrences
* [https://www.windowscentral.com/how-fix-update-causing-inaccessible-boot-device-error-windows-10](https://www.windowscentral.com/how-fix-update-causing-inaccessible-boot-device-error-windows-10)
* [https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-or-remove-packages-offline-using-dism](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-or-remove-packages-offline-using-dism)

Quick Tip: If your PC isn't booting from the USB flash drive, you'll need to change your system's BIOS settings to make sure it can boot from USB. Usually, you can access the BIOS by powering up your device and hitting one of the functions or ESC keys, but make sure to check your manufacturer's support website for more information.
Click Next.
Click the Repair your computer link in the bottom-right corner.
In the "Advanced startup" experience, click the Troubleshoot button.

![alt](https://www.windowscentral.com/sites/wpcentral.com/files/styles/large/public/field/image/2017/10/advanced-options-command-prompt-windows10.jpg?itok=Vk9c--8c)

Click the Command Prompt button.


Type the following commands to delete the SessionsPending Registry key and press Enter on each line:

``reg load HKLM\temp c:\windows\system32\config\software``

``reg delete "HKLM\temp\Microsoft\Windows\CurrentVersion\Component Based Servicing\SessionsPending"/v Exclusive``

``reg unload HKLM\temp``

Type the following command to pull a list of installed packages and press Enter:

``dism /image:C:\ /get-packages``

![alt](https://www.windowscentral.com/sites/wpcentral.com/files/styles/large/public/field/image/2017/10/DISMListofPackages_0.jpg?itok=nf_q6Ss0)

In the list of installed packages, look for those labeled "Install Pending," which are the ones likely causing the problem.

Type the following command to create a temporary folder to move the bad updates and press Enter:

``MKDIR C:\temp\packages``

Type the following command to remove the problematic updates and press Enter:

``dism /image:c:\ /remove-package /packagename:PACKAGE-IDENTITY-NAME /scratchdir:c:\temp\packages``

In the above command replace "PACKAGE-IDENTITY-NAME" for the package identify name labeled as "Install Pending" and repeat the step if you have more pending updates.

If the output looks similar to the screenshot below, then the update has been removed successfully, and you can restart your computer to boot normally again.

![alt](https://www.windowscentral.com/sites/wpcentral.com/files/styles/large/public/field/image/2017/10/dism_remove_package_oct10-update.jpg?itok=GhdLV0HI)
