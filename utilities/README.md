# Gotchas

## Docker For Mac pass through USB
- Using Docker for mac, it is [not possible to pass through a usb device to a container](https://docs.docker.com/desktop/faqs/#can-i-pass-through-a-usb-device-to-a-container). This means that we cannot necessarily have an appium server inside a container and instruct the device connected to host machine. This is because the support needs to be at hypervisor level. So what are the implications ?
    - Cannot set up a full development environment using Docker for mac and test things out
    - Using a docker-machine is a workaround
    - See the notebook [identify_tty_to_usbdevice_mac.ipynb](./identify_tty_to_usbdevice_mac.ipynb) for more details and progress

- There is also the command `system_profiler SPUSBDataType` that lists the USB devices, but it does NOT contain /dev/tty* value that we are looking for
- Another simple library that uses the system_profiler [lsusb](https://github.com/jlhonora/lsusb)

## docker-machine passthrough USB
- docker-machine was using VirtualBox and USB drivers had to be enabled , so had to install [VirtualBox Extensions](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1). I used 6.1.24 and it may change at the time you are reading it. Extension pack version had to exactly match the VirtualBox in my case for e.g. I tried to install Extension pack v6.1.26 on VirtuablBox v1.6.24, but that failed
- After Extension Pack was installed, had to go to Settings > Ports > USB and checkbox [x] Enable USB Controller and underneath radiobutton (.) USB 2.0 (OHCI + ECHCI) Controller (This is because I plugged my device into USB2.0 port on a USB extension hardware that had 2 USB2.0 ports and 2 USB 3.0 ports)
- USB Device needs to be unplugged BEFORE powering up the VM. on that requirement is met (and the filter created), as soon as you plug the device it pops-up on the running VM.
- Further the files `adbkey` & `adbkey.pub` in the physical host machine `~/.android/` had to be copied to `/home/docker/.android` (create the `.android` folder - it doesn't exist by default) and then mount that further into the container `$ docker run --privileged -d -p 4723:4723 -v ~/.android:/root/.android -v /dev/bus/usb:/dev/bus/usb --name container-appium appium/appium`