# Unix/Linux Kernel

## Unix/Linux Kernel

The GNU/Linux kernel is built mostly with the following programming languages:

* C
* Assembly

The GNU/Linux project can be found here:

{% embed url="https://github.com/torvalds/linux" %}

The documentation is here:

{% embed url="https://www.kernel.org/doc/html/latest/" %}

### Linux Kernel Modules (LKM)

Linux kernel rootkits are often developed using LKMs.

Kernel modules are reusable chunks of code that can be loaded and unloaded into the kernel at will. They allow the kernel to be extended without having to reset the system. A module's status can be set to built-in or loadable. A loadable module must be specified as a loadable module in the kernel configuration to dynamically load or uninstall a module.

There are a number of advantages to using kernel modules:

* Your kernel does not have to rebuild as frequently. This saves time and eliminates the risk of an error during the rebuilding and reinstallation of the basic kernel. It's best to leave a working base kernel alone for as long as possible once you've got it.
* It's a lot easier to figure out what's wrong with the system now. A flaw in a kernel-bound device driver can prevent the system from booting at all. It can often be difficult to determine which section of the basic kernel is causing the issue. However, if the same device driver is a module, the base kernel is already executing when the device driver is loaded. If the system crashes after the base kernel has started, it's simple to track the problem down to the problematic device driver and simply not load it until the problem is resolved.
* Modules can help preserve memory because they are only loaded when the system needs them. All of the base kernel's components are loaded in real storage, not virtual storage.
* Maintaining and debugging modules is significantly easier. What a filesystem driver integrated into the kernel would require a full reboot to accomplish may be accomplished with a few fast commands utilizing modules. Without having to wait for a boot, you can experiment with different parameters or even alter the code in fast succession.

#### Modules Location

The code necessary to create a new kernel with new module included or old modules removed is usually located at the following locations or paths:

* `/lib/modules/$(uname -r)/kernel`
* `/usr/lib/modules/$(uname -r)/kernel`

The `/usr/lib` and `/lib directory` are the paths where Linux stores object libraries and shared libraries that are necessary to run certain commands, including kernel code.

## Resources

{% embed url="https://tldp.org/HOWTO/Module-HOWTO/x73.html" %}
