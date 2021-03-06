# marz #
This allows any process to communicate with RIOT nativenet nodes on TAP interfaces
through local ports. This is not a border router but may help you in simple use cases.

## Installation & Dependencies
Marz depends on a patched development version of scapy. ``setup.sh`` will download the right version and apply the necessary patches automatically.
It also depends on Twisted (including Twisted-pairs) and bidict. You can get these with pip:
```
pip install --user Twisted
pip install --user bidict
```

## Usage
Marz reads the config file ``marz.config`` in the current directory.

```
[marz]
interface=tap0
hwaddr=256
panid=43981
```
It is preconfigured to use the IEEE 802.15.4 hardware address 0x100 and listen to the ``tap0`` interface (which you have to create beforehand and should be bridged to at least one RIOT nativenet instance). It will receive and send frames with the configured panid value (these are decimal values).

```
[UDP6:5683]
recipient=1
port=5683
```
This opens the local port UDP6 port 5683 and will relay every packet received to hardware address 1 on the same port.

Marz generates its own IP address by using ``ff02::1:0:ff00:`` + hwaddr, so the default address is ``ff02::1:0:ff00:100``. Recipient addresses are generated by ``fe80::ff:fe00:`` + recipient.

SixLoWPAN fragmentation only works one-way (from the outside to RIOT).
