# KVM
## Install KVM
### CentOS
- Refer to [these steps](doc/KVMandWindowsServerSetup.md#kvm-setup).

### Ubuntu
- [Install KVM on Ubuntu](doc/InstallKVMonUbuntu.md)

## Install Virtual Machines
### Windows
- [Install Windows OS](doc/KVMandWindowsServerSetup.md)

### Linux
- Red Hat Enterprise Linux and Clone OS
  - [Install Linux OS with CUI](doc/HowToInstallLinuxWithCUI.md)
- Ubuntu
  - From Ubuntu 22.04, you cannot get legacy type DVD so that you cannot use [the above steps](#how-to-install-linux-with-cui) to install Ubuntu. Please refer to the following documentation.
  - [Install Ubuntu with CUI](doc/HowToInstallUbuntuWithCUI.md)
- Amazon Linux
  - [Install Amazon Linux with CUI](doc/HowToInstallALWithCUI.md)
- Fedora
  - We cannot istall Fedora with iso file and virt-istall command. So, we use the qcow2 file to install Fedora.
  - https://docs.fedoraproject.org/en-US/fedora-server/virtualization/vm-install-diskimg-fedoraserver/#_instantiation_of_a_server_virtual_machine
