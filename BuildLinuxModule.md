# Building a Linux module

A minimal code for a kernel module is

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

On debain (and probably ubuntu) the following procedure can be followed to compile and load the module:

## Install the dependencies

```bash
apt-get install build-essential linux-headers-`uname -r`
```

Then create a Makefile for the module. A minimal one whould be:

```makefile
obj-m += hello.o

all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
