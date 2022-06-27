# How to setup KVM and Virtual Windows Server (CUI) to configure EXPRESSCLUSTER X with a shared disk.

## Introduction

- This guide provides how to setup KVM and Virtual Windows Server.
- Regarding shared disk settings, please refer to the following documents.
    - https://github.com/EXPRESSCLUSTER/iSCSI/blob/master/iSCSISetup.md


## System Configuration
- Linux OS: CentOS 7.4.1708
- Windows OS: Windows Server 2016 R4 (CUI)


- API to control virtual resources: libvirt 3.9.0
- Emulater: QEMU 1.5.3

- EXPRESSCLUSTER X 4.0


```bat
                   KVM VMs
  +----------------+    +----------------+
  | Windows Server |    | Windows Server |
  |                |    |                |
  |iSCSI initiator |    |iSCSI initiator |
  |      ECX       |    |      ECX       |
  |                |    |                |
  +------+---------+    +--------+-------+
         |                       |
         |      +----------------+
         |      |
  +------|------|------------------------+
  |  +---+------+---+                    |
  |  |virtual bridge|                    |
  |  +--------------+                    |
  |      |                               |
  |      |         Cent OS               |
  |      |                               |
  |      |       iSCSI target            |
  |      |         QEMU-KVM              |
  +------+-------------------------------+
         |         KVM Host
         |
<LAN>----+--------------------------------
```


## System setup
### KVM setup
1. Install some KVM softwares

    After installation of KVM, a virtual bridge "virbr0" of a virtual network "default" is created.
    
    When you create a new virtual network, a new virutal bridge is created.

    ```bat
    $ yum -y install libguestfs libvirt libvirt-client python-virtinst qemu-kvm virt-manager virt-top virt-viewer virt-who virt-install bridge-utils
    ```
    
    
2. Construct a virtual network

    First of all, launch the "virt-manager".
    
    You can construct virtual environments and operate virtual machines with virt-manager.

    ```bat 
    $ virt-manager
    ```
    
    1. Click "Edit" and then "Connection Details".
    
    2. Click "Virtual Networks" tab.
    
    3. Click "+"(Add Network) button.
    
    4. Type "Network Name" and then define its IP address.
        
        If you use DHCP, please check "Enable DHCPv4".
    
    5. Click "Forwarding to physical network".
    
        A virtual machine can connect outside of its host machine via its virtual bridge.
        
    6. Click "Finish".
    
3. Create virtual machines

    1. Click "File" and then "New Virtual Machine".

    2. "Local install media" and then select a ISO image.
    
    3. Define Memory, CPUs, Storages and the network you created in advance.
    
4. Edit a configuration of virtual machines

    You can edit a configuration of virtual machines by clicking "Open" after right-clicking servers.
    
    
### Windows setup
1. Disable firewall

    ```bat
    > netsh advfirewall set allprofiles state off
    ```
    
    or on PowerShell
    
    ```bat
    > PS C:> Get-NetFirewallProfile | Set-NetFirewallProfile -Enabled false
    ```
    
2. Configure computer name, network settings
	
	You can configure these settings using the "sconfig" command.
	
	```bat
	> sconfig
	```
	
3. Configure date and time

    ```bat
    > timedate.cpl
    ```
    
4. Configure region and language

    ```bat
    > intl.cpl
    ```

5. Configure disk settings

    ```bat
    > diskpart
    ```
    
## Setup EXPRESSCLUSTER X

Please look up how to setup EXPRESSCLUSTER X in the URL below.

- https://www.nec.com/en/global/prod/expresscluster/en/support/manuals.html

## Appendix

### Useful commands for Windows server

- Display NIC name

    ```bat
    > ipconfig
    ```

- Disable NIC

    ```bat
    PS C:> Disable-NetAdapter -Name "<NIC name>"
    
    e.g. > Disable-NetAdapter -Name "Ethernet 1"
    ```
    
- Enable NIC

    ```bat
    PS C:> Enable-NetAdapter -Name "<NIC name>"
    
    e.g. > Enable-NetAdapter -Name "Ethernet 1"
    ```
    
- Open multiple command-prompts

    1. Ctrl+Alt+Delete
    
    2. Select "Task Manager"
    
    3. Select "Run new task" after right clicking on "Windows Command Processor"
    
    4. Type "cmd" and then click "OK"
    

### References

- https://www.virment.com/kvm-configuration/
