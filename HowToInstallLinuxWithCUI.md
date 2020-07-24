# How to Install Linux with CUI
1. Create image file as below.
   ```sh
   # qemu-img create -f qcow2 rhel8.qcow2 60G
   ```
2. Run the following command to install Linux. 
   ```sh
   virt-install --name rhel8 --hvm --virt-type kvm --ram 1024 --vcpus 1 --arch x86_64 --os-type linux --disk /vm/rhel8/rhel8.qcow2 --network bridge=virbr0 --graphics none --serial pty --console pty --location rhel-8.2-x86_64-dvd.iso --extra-args "console=ttyS0"
   ```
   - If you can find your OS version, you can set it with ```--os-variant``` option.
     ```sh
     # osinfo-query os
      Short ID             | Name                                               | Version  | ID
     ----------------------+----------------------------------------------------+----------+-----------------------------------------
      alpinelinux3.5       | Alpine Linux 3.5                                   | 3.5      | http://alpinelinux.org/alpinelinux/3.5
      :
      rhel7.8              | Red Hat Enterprise Linux 7.8                       | 7.8      | http://redhat.com/rhel/7.8
      :
     ```
3. You can get installation CUI.