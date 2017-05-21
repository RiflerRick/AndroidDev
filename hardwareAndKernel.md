### Hardware 

Basic sensors

### Android Linux Kernel

Andriod Linux is actually a variant of the standard linux kernel, written in C and ships separately from the rest of the android stack. It shields higher level android layers from the lower levels basically providing abstraction. Mediates access to and shares various hardware present inside the device.

Android linux kernel is a forked version of the GNU linux kernel and hence are not identical.

### MiddleWare

Android's middleware infrastructure provides reusable capabilities that extend hardware centric OS kernel and protocol mechanisms. The hardware abstraction layer(HAL) is used to shield upper levels from lower levels. The wifi, bluetooth, radio, gps etc are all handled using the HAL. 

If we write a device driver in a Linux kernel its source code must be released. Not all OEMs like to do that so HAL dictates that its source code if written may not be released and is licensed in a different way.

#### Runtime and Libraries

**Android Runtime**

- A managed execution env- Run efficiently java based apps and some other system services. In early versions it was the Darvik vitual machine which has now been replaced by the ART(Android runtime). It uses what is called an ahead of time compilation model rather than a just in time compilation model and a virtual machine model. It is optimized.

**Andriod Libraries**

Libraries written in java specifically designed for android.

**Native C/C++ libraries**

Androids core libraries provide wrapper facades to the C/C++ libraries. Wrapper facades are simply the java binding to the C/C++ libraries.



