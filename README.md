# arducam_dkms

kernel module and device tree overlay to add support for the Arducam Pivariety V4L2 Driver on the Raspberry Pi Compute Module 4 IO Board.

*Works with 5.10+ 64-bit kernels only.*

## Usage
1. Install dkms if you haven't already:
```
sudo apt install dkms raspberrypi-kernel-headers --no-install-recommends
```
2. Download the latest source .tar.gz from the [releases](https://github.com/gardinltd/arducam_dkms/releases/) page
3. Untar it to `/usr/src/arducam-<version>` and run the dkms install:
```
tar -xzvf 1.0.0.tar.gz -C /usr/src/
sudo dkms install arducam/1.0.0
```
4. Add these lines to your /boot/config.txt and reboot.
   See below for more config options.
```
# Enable I2C bus 1 on VideoCore (/dev/i2c-10 in Raspberry Pi OS)
dtparam=i2c_vc=on
# Enable arducam driver
dtoverlay=arducam,media-controller=0
```
5. Some distributions may not automatically load the kernel module despite the devicetree entry; Raspberry Pi OS does, Ubuntu 21.10 does not, etc. 
   To make the module load, edit `/etc/modules` (or make a new file called `/etc/modules-load.d/arducam.conf`), adding this line:
```
arducam
```

## Install from git
1. Install dkms if you haven't already:
```
sudo apt install dkms raspberrypi-kernel-headers --no-install-recommends
```
2. Clone the repo
```
git clone https://github.com/gardinltd/arducam_dkms.git
cd arducam_dkms
```
3. Run install.sh, feel free to inspect it yourself first. It will archive the current HEAD to /usr/src with an appropriate version, and run DKMS.


## Config options
The device tree overlay has a few options, here's the equivalent of a `/boot/overlays/README` info section:

```
Name:   arducam
Info:   Arducam Pivariety V4L2 camera module.
        Uses Unicam 1, which is the standard camera connector on most Pi
        variants.
Load:   dtoverlay=arducam,<param>[=<val>]
Params: rotation                Mounting rotation of the camera sensor (0 or
                                180, default 180)
        orientation             Sensor orientation (0 = front, 1 = rear,
                                2 = external, default external)
        media-controller        Configure use of Media Controller API for
                                configuring the sensor (default on)
        cam0                    Adopt the default configuration for CAM0 on a
                                Compute Module (CSI0, i2c_vc, and cam0_reg).
```