# How to Install Amazon Linux with CUI
## Amazon Linux 2023
- I'm waiting for the virtual machin image...

## Amazon Linux 2
- Reference
  - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html
- I have run the following command to install Amazon Linux 2.
  ```
  virt-install \
  --name al2-153 \
  --hvm \
  --virt-type kvm \
  --ram 1024 \
  --vcpus 1 \
  --arch x86_64 \
  --os-type linux \
  --disk /vm1/al2-153/al2-153.qcow2 \
  --network bridge=virbr0 \
  --graphics none \
  --serial pty \
  --console pty \
  --cdrom /vm1/al2-153/seedconfig/seed.iso
  ```

### Upgrade Kernel
1. Check the current kernel version.
   ```
   [ec2-user@al2-153 ~]$ uname -r
   4.14.311-233.529.amzn2.x86_64
   ```
1. If you have a proxy server, create the following file and edit as below.
   ```sh
   sudo vim /etc/amazon-linux-extras.conf
   ```
   ```
   export http_proxy=<your proxy server>:<port number>
   export https_proxy=<your proxy server>:<port number>
   ```
1. Run the following command.
   ```sh
   sudo amazon-linux-extras |grep kernel
   ```
   ```
    49  kernel-5.4               available    [ =stable ]
    55  kernel-5.10              available    [ =stable ]
    62  kernel-5.15              available    [ =stable ]
   ```
1. Install kernel-5.10.
   ```sh
   sudo amazon-linux-extras install kernel-5.10 -y
   ```
1. Check the status.
   ```sh
   sudo amazon-linux-extras |grep kernel
   ```
   ```
    _  kernel-5.4               available    [ =stable ]
   55  kernel-5.10=latest       enabled      [ =stable ]
   62  kernel-5.15              available    [ =stable ]
   ```
1. Check the default kernel with grubby command.
   ```
   [ec2-user@al2-153 ~]$ sudo grubby --default-index
   0
   ```
   ```
   [ec2-user@al2-153 ~]$ sudo grubby --info=ALL
   index=0
   kernel=/boot/vmlinuz-5.10.177-158.645.amzn2.x86_64
   args="ro  console=ttyS0,115200n8 console=tty0 net.ifnames=0 biosdevname=0 nvme_core.io_timeout=4294967295 LANG=en_US.UTF-8 KEYTABLE=us"
   root=UUID=1e3c813c-c441-4ec0-b45b-c757ec21c19d
   initrd=/boot/initramfs-5.10.177-158.645.amzn2.x86_64.img
   title=Amazon Linux (5.10.177-158.645.amzn2.x86_64) 2
   index=1
   kernel=/boot/vmlinuz-4.14.311-233.529.amzn2.x86_64
   args="ro  console=ttyS0,115200n8 console=tty0 net.ifnames=0 biosdevname=0 nvme_core.io_timeout=4294967295"
   root=UUID=1e3c813c-c441-4ec0-b45b-c757ec21c19d
   initrd=/boot/initramfs-4.14.311-233.529.amzn2.x86_64.img
   title=Amazon Linux (4.14.311-233.529.amzn2.x86_64) 2
   index=2
   kernel=/boot/vmlinuz-4.14.311-233.529.amzn2.x86_64
   args="ro single"
   root=UUID=1e3c813c-c441-4ec0-b45b-c757ec21c19d
   initrd=/boot/initramfs-4.14.311-233.529.amzn2.x86_64.img
   title=Amazon Linux (4.14.311-233.529.amzn2.x86_64) 2 (recovery mode)
   index=3
   non linux entry   
   ```
1. Reboot OS.
   ```sh
   sudo reboot
   ```
1. Check the current kernel.
   ```
   [ec2-user@al2-153 ~]$ uname -r
   5.10.177-158.645.amzn2.x86_64
   ```
