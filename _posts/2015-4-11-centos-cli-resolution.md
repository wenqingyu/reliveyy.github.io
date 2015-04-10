---
layout: post
title: Change CentOS 7 CLI Resolution in VMware Fusion
category: tricks
comments: true
tags: Linux
---

Open the file `/etc/default/grub`, add

```
GRUB_CMDLINE_LINUX_DEFAULT="splash vga=33C"
```

Don't be panic if you use a wrong vga mode above. Because you have a chance to choose another vga mode when booting. 

Then update grub in CentOS like this

```
grub2-mkconfig -o "$(readlink /etc/grub2.cfg)"
```

Finally, reboot your system, the resolution will be changed.
