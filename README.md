# Raspberrypi

The Raspberry Pi 5 includes 5 active PCI Express lanes—4 go to the new RP1 chip for I/O like USB, Ethernet, MIPI Camera and Display, and GPIO, and 1 goes to a new external PCIe connector:

Raspberry Pi 5 PCIe connector

By default, all PCIe lanes operate at Gen 2.0 speeds, or about 5 GT/sec per lane. Currently there's no way to change that default for the RP1 chip's 'internal' lanes, but on the external connector, you can add the following lines inside /boot/firmware/config.txt (and reboot) to upgrade the connection to Gen 3.0 (8 GT/sec, almost double the speed):

dtparam=pciex1
dtparam=pciex1_gen=3

And yes, you can also downgrade the connection to Gen 1.0 speeds (2.5 GT/sec) if you like.
Why default to PCIe Gen 2.0?

Why is it defaulted to Gen 2.0? Because that's the speed at which the board could be certified for PCI Express. Even older standards like 2.0 and 3.0 are considered 'high speed' interconnects. And with any connection on a board, interference and signal issues can cause problems with higher bandwidth.

On expensive motherboards, PCIe Gen 5 and even Gen 4 have issues with some configurations, especially if people use risers for things like vertically-mounted GPUs.

Raspberry Pi 5 PCIe external FPC Connector - Flat cable

But even on the tiny Pi 5, things like the insertion loss from the flat FPC connection can cause issues at higher speeds. I encountered some link errors from time to time, and they were compounded on certain devices which don't handle them as gracefully.

One such device was the Google Coral TPU, which seemed to like resetting its connection to the Pi 5 constantly, to the point I can't get it working yet (under Gen 1, 2, or 3!). You can follow the saga here, but other devices do see a massive benefit with Gen 3.0 speeds:

    My 10G ASUS Network Card now gets 6 Gbps (instead of about 3.5) with the Pi 5.
    My Kioxa XG8 NVMe SSD now gets 900 MB/sec reads versus 450 MB/sec under Gen 2.0.

It'd be really interesting to see if we can hack a Pi 5 board to expose all five lanes of PCIe from the BCM2712, and uprate all of them to Gen 3. You could conceivably run a 10 Gbps NAS or multi-port 2.5 Gbps router or firewall pretty easily with that bandwidth, sucking down 2-3W at idle.
