# ifconfig
***
## Basic
**ifconfig** - утилита конфигурирует сетевой интерфейс устройста.
### Syntax
``` bash
ifconfig [-v] [-a] [-s] [interface]
ifconfig [-v] interface [aftype] options | address ...
```
***
## Description
**Ifconfig** используется для настройки резидентных сетевых интерфейсов ядра. Он используется во время загрузки для настройки интерфейсов по мере необходимости. После этого он обычно требуется только при отладке или настройке системы. 
Если аргументы не указаны, **ifconfig** отображает состояние активных в данный момент интерфейсов.
***
## Options
- `-a` - display all interfaces which are currently available, even if down
- `-s` - display a short list (like `netstat -i`)
- `-v` - be more verbose for some error conditions
- `interface` - The  name  of the interface.  This is usually a driver name followed by a unit number, for example `eth0` for the first Ethernet interface. If your kernel supports alias interfaces, you can specify them with syntax like `eth0:0` for the first alias of `eth0`. You can use them to assign more addresses. To delete an alias interface use `ifconfig eth0:0 down`.  Note: for every scope (i.e. same net with address/netmask combination) all aliases are  deleted, if you delete the first (primary).
- `up` - This  flag  causes  the  interface  to  be  activated.  It is implicitly specified if an address is assigned to the interface; you can suppress this behavior when using an alias interface by appending an - to the alias (e.g. `eth0:0-`).  It is also suppressed when using the `IPv4 0.0.0.0` address as the kernel will use this to implicitly delete alias interfaces.
- `down` - This flag causes the driver for this interface to be shut down.
	- `arp` - Enable or disable the use of the ARP protocol on this interface.
	- `promisc` - Enable or disable the promiscuous mode of the interface.  If selected, all packets on the network will be received by the interface.
	- `allmulti` - Enable or disable all-multicast mode.  If selected, all multicast packets on the network will be received by the interface.
- `mtu N` - This parameter sets the Maximum Transfer Unit (MTU) of an interface.
- `dstaddr [address]` - Set the remote IP address for a point-to-point link (such as PPP).  This keyword is now obsolete; use the pointopoint keyword instead.
- `netmask [address]` - Set the IP network mask for this interface.  This value defaults to the usual class A, B or C network mask (as derived from the interface IP address), but it can be set to any value.
- `add [address]/[prefix_length]` - Add an IPv6 address to an interface.
- `del [address]/[prefix_length]` - Remove an IPv6 address from an interface.
- `tunnel ::aa.bb.cc.dd` - Create a new SIT (IPv6-in-IPv4) device, tunnelling to the given destination.
- `irq [address]` -Set the interrupt line used by this device.  Not all devices can dynamically change their IRQ setting.
- `io_addr [address]` - Set the start address in I/O space for this device.
- `mem_start [address]` - Set the start address for shared memory used by this device.  Only a few devices need this.
- `media type` - Set the physical port or medium type to be used by the device.  Not all devices can change this setting, and those that can vary in what values they support.  Typical values for type  are  10base2  (thin  Ethernet),  10baseT (twisted-pair 10Mbps Ethernet), AUI (external transceiver) and so on.  The special medium type of auto can be used to tell the driver to auto-sense the media.  Again, not all drivers can do this.
	- `broadcast [address]` -If the address argument is given, set the protocol broadcast address for this interface.  Otherwise, set (or clear) the IFF_BROADCAST flag for the interface.
	- `pointopoint [address]` - This keyword enables the point-to-point mode of an interface, meaning that it is a direct link between two machines with nobody else listening on it. If the address argument is also given, set the protocol address of the other side of the link, just like the obsolete dstaddr keyword does.  Otherwise, set or clear the IFF_POINTOPOINT flag for the interface.
- `hw class [address]` - Set  the hardware address of this interface, if the device driver supports this operation.  The keyword must be followed by the name of the hardware class and the printable ASCII equivalent of the hardware address.  Hardware         classes currently supported include ether (Ethernet), ax25 (AMPR AX.25), ARCnet and netrom (AMPR NET/ROM).
- `multicast` - Set the multicast flag on the interface. This should not normally be needed as the drivers set the flag correctly themselves.
- `address` - The IP address to be assigned to this interface.
- `txqueuelen [length]` - Set the length of the transmit queue of the device. It is useful to set this to small values for slower devices with a high latency (modem links, ISDN) to prevent fast bulk transfers from disturbing interactive traffic  like telnet too much.
- `name [newname]` - Change the name of this interface to newname. The interface must be shut down first.
***
