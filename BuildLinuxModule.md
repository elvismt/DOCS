# Building a Linux module

On debain (and probably ubuntu) the following procedure can be followed to compile and load the module

## Install the dependencies

In order to build a kernel module we need to install the kernel development headers and make sure we have a compiler

```bash
apt-get install build-essential linux-headers-`uname -r`
```

# Write the module

The minimal code for a kernel module is. name it `hello.c`.

```c
#include <linux/module.h>    /* included for all kernel modules */
#include <linux/kernel.h>    /* included for KERN_INFO */
#include <linux/init.h>      /* included for __init and __exit macros */

MODULE_LICENSE("GPL");
MODULE_AUTHOR("elvismt");
MODULE_DESCRIPTION("A Hello World module");

static int __init hello_init(void)
{
    printk(KERN_INFO "Hello world!\n");
    return 0; /* As in the usual main functions non zero means error */
}

static void __exit hello_cleanup(void)
{
    printk(KERN_INFO "Hello module is gone.\n");
}

module_init(hello_init);
module_exit(hello_cleanup);
```

##  Build

Create a Makefile for the module. A minimal one whould be:

```makefile
obj-m += hello.o

all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

Then type `make` in the terminal to actually comile your module.

## Load the module and check it loaded with success

To load a module you will need root privileges. Run

```bash
sudo insmod hello.ko
```

nothing should be printed, that's because the modules ouput is in the system log, not the terminal.
To see this output use the `dmesg` command

```bash
sudo dmesg | tail
```

This should show `Hello World` as the last message. To unload the driver use the rmmod command

```bash
sudo rmmod hello
```

`dmesg` can be used again to check for the goodbye message.
