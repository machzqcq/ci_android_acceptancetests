# Gotchas

- Using Docker for mac, it is [not possible to pass through a usb device to a container](https://docs.docker.com/desktop/faqs/#can-i-pass-through-a-usb-device-to-a-container). This means that we cannot necessarily have an appium server inside a container and instruct the device connected to host machine. This is because the support needs to be at hypervisor level. So what are the implications ?
    - Cannot set up a full development environment using Docker for mac and test things out
    - Using a docker-machine is a workaround