#About
This repository contains code that provides a working PXE server (via HTTP, TFTP, DHCP, and/or iPXE) implemented purely in Python. Currently, only Python2 is supported.

**DISCLAIMER:** None of these servers are fully compliant with any standards or specifications. However, the true specifications and standards were followed when building PyPXE and while they work for PXE any other uses are purely coincidental. Use at your own risk.

##Usage

###Using PyPXE as a Library
Each server type (TFTP/HTTP/DHCP) is in it's own class in it's own file and can be used independently if so desired. For more information on how each service works and how to manipulate them, see  ```DOCUMENTATION.md``` in the root of ```master```.

###QuickStart
```server.py``` uses all three services in combination with the option of enabling/disabling them individually while also setting some options. Edit the ```server.py``` settings to your preferred settings or run with ```--help``` or ```-h``` to see what command line arguments you can pass. Treat the provided ```netboot``` directory as ```/tftpboot/``` that you would typically see on a TFTP server, put all of your network-bootable files in there and setup your menu(s) in ```pxelinux.cfg/default```.

Simply run the following command and you will have an out-of-the-box PXE-bootable server that runs TFTP and serves files out of the ```netboot``` directory!
```shell
sudo python server.py
```
If you require the ability to handle DHCP PXE requests then you can either enable the built-in DHCP server (after configuring, of course)...
```shell
sudo python server.py --dhcp
```
...or start ```server.py``` in ProxyDHCP mode rather than a full DHCP server to prevent DHCP conflicts on your network...
```shell
sudo python server.py --dhcp-proxy
```

**TFTP/DHCP/HTTP/iPXE Arguments**

Enable iPXE ROM [Default: False]
```--ipxe```

Enable built-in HTTP server [Default: False]
```--http```

Enable built-in DHCP server [Default: False]
```--dhcp```

Enable built-in DHCP server in proxy mode (implies ```--dhcp```) [Default: False]
```--dhcp-proxy```

**DHCP Server Arguments**

Specify DHCP server IP address [Default: 192.168.2.2]
```-s DHCP_SERVER_IP``` or ``` --dhcp-server-ip DHCP_SERVER_IP```

Specify DHCP fileserver IP address [Default: 192.168.2.2]
```-f DHCP_FILESERVER_IP``` or ```--dhcp-fileserver-ip DHCP_FILESERVER_IP```

Specify DHCP lease range start [Default: 192.168.2.100]
```-b DHCP_OFFER_BEGIN``` or ```--dhcp-begin DHCP_OFFER_BEGIN```

Specify DHCP lease range end [Default: 192.168.2.150]
```-e DHCP_OFFER_END``` or ```--dhcp-end DHCP_OFFER_END```

Specify DHCP subnet [Default: 255.255.255.0]
```-n DHCP_SUBNET``` or ```--dhcp-subnet DHCP_SUBNET```

Specify DHCP lease router [Default: 192.168.2.1]
```-r DHCP_ROUTER``` or ```--dhcp-router DHCP_ROUTER```

Specify DHCP lease DNS server [Default: 8.8.8.8]
```-d DHCP_DNS``` or ```--dhcp-dns DHCP_DNS```

Specify the local directory where network boot files will be served [Default: 'netboot']
```-a NETBOOT_DIR``` or ```--netboot-dir NETBOOT_DIR```

Specify the PXE boot file name [Default: _automatically set based on what services are enabled or disabled, see documentation for further explanation_]
```-i NETBOOT_FILE``` or ```--netboot-file NETBOOT_FILE```

##Additional Information
```Core.iso``` located in ```netboot``` is from the [TinyCore Project](http://distro.ibiblio.org/tinycorelinux/) and is provided as an example to network boot from using PyPXE
```chainload.kpxe``` located in ```netboot``` is the ```undionly.kpxe``` from the [iPXE Project](http://ipxe.org/)  
```pxelinux.0```, ```menu.c32``` and ```memdisk``` located in ```netboot``` are from the [SYSLINUX Project](http://www.syslinux.org/)  

##ToDo
- Add ```--debug``` prints to dhcp/tftp/http (such as 404, Offer/ACKs w/ filename)
- [PEP8](http://legacy.python.org/dev/peps/pep-0008/)
- Turn longer functions to kwargs vs positional
- Remove hardcoded /24 in dhcpd