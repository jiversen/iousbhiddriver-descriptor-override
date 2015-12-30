IOUSBHIDDriverDescriptorOverride is a kernel extension that allows customization of the HID descriptor reported by USB HID devices to the Mac OS X HID system.

Perhaps an example is in order. The version of the [Griffin PowerMate](http://www.griffintechnology.com/products/powermate/) I own sends it's rotational data as relative (meaning it sends "-1", "+2", etc. each time it's twisted), but it's HID descriptor reports it as absolute data (meaning a bunch of -1's over time would be interpreted as "no movement" instead of "turning slowly to the left"). Unfortunately, this cannot be corrected at the application level because the HID system does not report the "-1" repeatedly: it just stays at "-1" until the PowerMate sends something else, and applications can't know if the PowerMate has stopped turning or not. But with IOUSBHIDDriverDescriptorOverride, the faulty bit of the HID descriptor can be corrected and the device will function as expected.

To customize the HID descriptor of a USB device, edit the IOKitPersonalities key of the kext's Info.plist. In addition to the normal matching data, a the custom HID descriptor for each device is provided with the data key "ReportDescriptorOverride". Examples are included. See http://www.usb.org/developers/devclass_docs/HID1_11.pdf for a general description of HID descriptors, http://www.usb.org/developers/devclass_docs/Hut1_12.pdf for a list of the HID Usages (opcodes), and use the USB Prober application in the Mac OS X developer tools to see USB devices' raw HID descriptors as well as the parsed descriptor.

Currently, the Info.plist contains the aforementioned PowerMate modification and one for the Macally iShock (on version 1 of the iShock, the joystick code is incorrect).