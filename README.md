asus-xtion-fix
==============

For making the Asus Xtion work on USB 3.0 on Linux (e.g. for [PCL](http://pointclouds.org)).

The Xtion apparently has a firmware problem that makes it not work on USB 3.0 ports.

You might get `dmesg` messages like:

```
[ 3162.597428] usb 3-3: new high-speed USB device number 2 using xhci_hcd
[ 3162.617432] usb 3-3: New USB device found, idVendor=1d27, idProduct=0600
[ 3162.617440] usb 3-3: New USB device strings: Mfr=2, Product=1, SerialNumber=0
[ 3162.617444] usb 3-3: Product: PrimeSense Device
[ 3162.617447] usb 3-3: Manufacturer: PrimeSense
[ 3162.617912] xhci_hcd 0000:00:14.0: Not enough bandwidth. Proposed: 1663, Max: 1607
[ 3162.617920] xhci_hcd 0000:00:14.0: Not enough bandwidth
[ 3162.617933] usb 3-3: can't set config #1, error -12
```

[Some sources](http://www.pcl-developers.org/xhci-hcd-I-hate-you-USB-3-0-and-Primesense-Asus-Xtion-td5707949.html)
recommended to blacklist the USB 3.0 driver `xhci_hcd` and use the
USB 2.0 driver `ehci_hcd` only.
(On e.g. Ubuntu you have to recompile the kernel for this, setting
these drivers to be built as modules instead of built into the kernel,
changing `CONFIG_USB_XHCI_HCD` and `CONFIG_USB_XHCI_HCD` from `y` to `m`).

However, this did not work for my computer, and after unloading the USB 3.0 driver,
the device was not recognized at all in `dmesg`.

A firmware update helped, described below.

Note, however, that the newer firmware does not seem to work
with [OpenNI 1](https://github.com/OpenNI/OpenNI).
[OpenNI 2](https://github.com/OpenNI/OpenNI2) **works** with it,
now finally on USB 3.0 on Linux, and its `NiViewer` can display the camera images.

Note that at the time of writing, PCL only supports OpenNI 1,
but there is a [pull request](https://github.com/PointCloudLibrary/pcl/pull/276)
for it to use OpenNI 2.


Upgrading the Xtion firmware
----------------------------

You have to flush the firmware from a Windows OS.

Unpack the [FW579-RD1081-112v2.zip](FW579-RD1081-112v2.zip) ZIP file, go into `UsbUpdate`, and run the file
`!Update-RD108x!.bat` from `cmd.exe` with Administrator privileges.

Extract the whole ZIP file, the files in the `FLA` folder are the
actual firmware.

You should see an output like in the file [firmware-flush-hotfix-log.txt](firmware-flush-hotfix-log.txt).


Original source
---------------

https://my.artec3d.com/kb/upgrading_asus_firmware


Related posts about the problem
-------------------------------

* http://www.pcl-developers.org/xhci-hcd-I-hate-you-USB-3-0-and-Primesense-Asus-Xtion-td5707949.html
* http://answers.ros.org/question/77651/asus-xtion-on-usb-30-ros-hydro-ubuntu-1210/
* https://groups.google.com/forum/#!topic/openni-dev/2Zo80pwfKU4
* http://askubuntu.com/questions/276425/asus-xtion-pro-live-not-working-with-xhci-hcd
