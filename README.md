# Wireguard
Wireguard kernel module and userspace-tools for asus routers running Entware 3.10

# Important notice
For the kernel modules to work properly you need to load one that is matching your kernel.
I have choose to keep @odkrys notation and use kernel sub-version as a suffix.

Only RT-AC86U / GT-AC2900 (4.1.27) and RT-AX88U / GT-AX11000 (4.1.51) is available at this time.

# Userspace-tools
This is Firmware independent and will be installed automatically by Wireguard Session Manager.
If you for some reason want to install this yourself, use this to download to your router:
```sh
curl -LJO https://raw.githubusercontent.com/ZebMcKayhan/Wireguard/main/wireguard-tools_1.0.20210914-1_aarch64-3.10.ipk
```

# RT-AC86U / GT-AC2900
kernel 4.1.27  
wireguard-kernel_1.0.20211208-RT-AC86U_2_aarch64-3.10.ipk    
wireguard-kernel_1.0.20211208-GT-AC2900_2_aarch64-3.10.ipk  
wireguard-kernel_1.0.20211208-k27_2_aarch64-3.10.ipk  

# RT-AX88U / GT-AX11000
kernel 4.1.51  
wireguard-kernel_1.0.20211208-RT-AX88U_2_aarch64-3.10.ipk  
wireguard-kernel_1.0.20211208-GT-AX11000_2_aarch64-3.10.ipk  
wireguard-kernel_1.0.20211208-k51_2_aarch64-3.10.ipk  

# Installation
If your system matches the kernel then this will be installed automatically by Wireguard Session Manager.
If you for some reason want to install this yourself, use this to download to your router:
```sh
curl -LJO https://raw.githubusercontent.com/ZebMcKayhan/Wireguard/main/wireguard-kernel_1.0.20210606-k27_1_aarch64-3.10.ipk
```

If you don't know how to install this you probably shouldn't try. Instead follow the link below to install Wireguard Session Manager and it will install it and setup your system properly.
# Informational sources
[Ubuntu 20.04 LTS - Windows store](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71?activetab=pivot:overviewtab)

[Firmware compilation WSL2 setup](https://github.com/RMerl/asuswrt-merlin.ng/wiki/Compiling-under-WSL2)

[Firmware compilation](https://github.com/RMerl/asuswrt-merlin.ng/wiki/Compile-Firmware-from-source-using-Ubuntu)

[SNB Forum - amcfwm - AsusWRT-Merlin Custom Firmware Manager](https://www.snbforums.com/threads/amcfwm-asuswrt-merlin-custom-firmware-manager.63227/)

[Wireguard/Tools makefiles and Entware compilation](https://github.com/odkrys/entware-makefile-for-merlin)

[Wireguard-kernel backports](https://git.zx2c4.com/wireguard-linux-compat)

[Wireguard-tools backports](https://git.zx2c4.com/wireguard-tools)

[SNB Forum - [Experimental] WireGuard for HND platform (4.1.x kernels)](https://www.snbforums.com/threads/experimental-wireguard-for-hnd-platform-4-1-x-kernels.46164/)

[SNB Forum - Wireguard Session Manager](https://www.snbforums.com/threads/session-manager-discussion-thread-closed-expired-oct-2021.70787/)

[SNB Forum - Wireguard Session Manager (2nd Thread)](https://www.snbforums.com/threads/session-manager-discussion-2nd-thread.75129/)

# Compilation process

Download and install "ubuntu 20.04 LTS" from windows store

Setup the program according to "Firmware compilation WSL2 setup"

Compile your firmware according to "Firmware compilation Ubuntu"

optionally install and use "amcfwm - AsusWRT-Merlin Custom Firmware Manager" whichever works best.

Setup Entware buildroot according to "Wireguard/Tools makefiles and Entware compilation"

If/when any compilation fails, use "make J 1 V=s ..." to find out what is going wrong. usually web pages returning 404 so just try again.

place the makefiles in its directories here:
```sh
/USERNAME/Entware/feeds/packages/net/wireguard-kernel
/USERNAME/Entware/feeds/packages/net/wireguard-tools
```

Edit the kernel makefile and change this to point to were you built the firmware
```sh
export LINUX_DIR:=/USERNAME/amng-build/release/src-rt-5.02hnd/kernel/linux-4.1
```

Also change this to point to the release of your choice (see "Wireguard-kernel backports" link)
```sh
PKG_VERSION:=1.0.20200908
PKG_RELEASE:=ac
PKG_HASH:=7c0e576459c6337bcdea692bdbec561719a15da207dc739e0e3e60ff821a5491
```
if you cant get the hash, leave the old hash and the compiler will later complain about it, and gives you the proper hash to fill in.

The PKG_RELEASE is only used as a suffix name on the output file, so change according to your build/needs.

Create the symlink by executing:
```sh
ln -s /USERNAME/Entware/feeds/packages/net/wireguard-kernel /USERNAME/Entware/package/feeds/packages
ln -s /USERNAME/Entware/feeds/packages/net/wireguard-tools /USERNAME/Entware/package/feeds/packages
```

Create configuration:
```sh
cd /USERNAME/Entware
make oldconfig 
```
Choose M as modular.

Compile the sources:
```sh
make package/wireguard-kernel/compile V=s
make package/wireguard-tools/compile V=s
```

The compiled packages will appear here:
```sh
/USERNAME/Entware/bin/targets/aarch64-3.10/generic-glibc/packages
```
