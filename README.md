# BOLYFA117-USB-DMM
USB serial interpreter for the BOLYFA 117 Digital MultiMeter (https://amzn.to/2Hq01Nm)

I love this multimeter; its look, it's feel, the fab screen & backlight, and quite importantly, the price - it's in that perfect, let's give it a chance - it's inexpensive enough that even if the USB functionality is no good, I still have a decent looking & primarily functional multimeter.

I would very much like to use the USB connection to log the data from the multimeter, but the support for this is somewhat lacking. I tried to run the installer on my Windows 10 VM, but the provided software is old and doesn't support modern Windows OS versions.

However, I am a Linux user & system administrator, so I'd prefer if I could use it natively in Linux. I looked through all of the files on the driver CD and found (inside one of the configuration files, deep within the file structure - not in any actual documentation), that the USB serial connection is at 2400 baud (& guessed at 8 bits, No parity, and 1 Stop bit).

On my desktop Linux workstation I have managed to serially connect to & can see output from the device, but it is not easily decipherable by the human eye.  Here are some CLI commands & output, showing my experience with the USB connection: -

# My PC's detail
% uname -a
Linux aphrodite 5.4.0-7642-generic #46~1598628707~20.04~040157c-Ubuntu SMP Fri Aug 28 18:02:16 UTC  x86_64 x86_64 x86_64 GNU/Linux

# Watch Linux see it handle the insertion of the new USB device & if it recognises the hardware (it does) & attaches a driver (it did)
% dmesg -w
usb 3-2.4: new full-speed USB device number 66 using xhci_hcd
usb 3-2.4: New USB device found, idVendor=1a86, idProduct=7523, bcdDevice= 2.54
usb 3-2.4: New USB device strings: Mfr=0, Product=2, SerialNumber=0
usb 3-2.4: Product: USB2.0-Serial
ch341 3-2.4:1.0: ch341-uart converter detected
usb 3-2.4: ch341-uart converter now attached to ttyUSB0

# View the USB device and driver info
% sudo lsusb -s 66
Bus 003 Device 066: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter

# Check ownership of the USB device
% ll /dev/ttyUSB0
crw-rw----+ 1 root dialout 188, 0 Oct 23 11:27 /dev/ttyUSB0

# Add my user to the group (so that I can access it without elevated privileges)
% sudo usermod -a -G dialout myuser

# Connect to the USB via serial (Ctrl-A K Y, to quit)
% screen /dev/ttyUSB0 2400
#
# Serial output while the LCD is reading: 15.2 Ohms
�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=  @�UR$�=
#
# Serial output while the LCD is reading: 25°C
�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k�UR$=k

Not ideal, so far!  I would like to help get this working in my Linux environment & wondered if anyone could please share with me, how I can interpret the output from the device, so that I may find a way to make the data logging feature actually useful. I would be happy to OpenSource my final result (github.com/furriephillips/BOLYFA117-USB-DMM) so that anyone else in the Linux community knows that this multimeter is compatible and potentially the preferred device for users of the Linux OS. Alternatively, it'd be great if the manufacturer could OpenSource their Windows software & perhaps get help from the wider GitHub community, to improve support for any OS flavour...
