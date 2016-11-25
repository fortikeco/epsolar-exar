#epsolar-exar
EPSolar Tracer A series, CC-USB-RS485-150U, Exar RS485 to USB adapter, USB UART driver for linux kernel 3.4.x
=============================================================================================================
The EPSolar Tracer A series MPPT solar charger uses a ModBus protocol over UART (USB).
This fix is not about a solution for using the specific EPSolar ModBus protocol, but for the USB adapter.
If one connects the device with the USB-RS485 adapter (which can be ordered separately from the EPSolar) and do a lsusb, it shows up as an Exar device.

History of the driver
--------------------
The problem is that there is no compatible USB UART driver anymore for this device in the linux kernel "mainline" tree.
It used to be in the mainline (known as vizzini, by Ravi Reddy), but the linux kernel developers removed it from the tree, I think probably because of miscommunication.

Exar states on https://www.exar.com/design-tools/software-drivers that they have a driver for linux kernel up to 3.4.x.
(The USB UART driver for "Linux 2.6.18 to 3.4.x")

There exists no driver for specific the 3.4.x kernel, due to fundamental linux usb-serial API changes.

Specifically, the following commit on the kernel added a new usb-serial API:
https://github.com/torvalds/linux/commit/765e0ba62613fb90f09c1b5926750df0aa56f349

The following deleted the old API (and broke the Exar code to link against kernel 3.4.x)
https://github.com/torvalds/linux/commit/f799e7678390029e322ae2dc3cda389b11f38124

And, again the API changed in 3.5.x kernels:
https://github.com/torvalds/linux/commit/68e24113457e437b1576670f2419b77ed0531e9e

Download source code
--------------------
So for the people that need persee 3.4.x compatibility (specific ARM boards with closed-source MALI drivers):
[here is the changed source using the 3.4.x usb-serial API](xr_usb_serial_common-linuxkernel-3.4.x).
