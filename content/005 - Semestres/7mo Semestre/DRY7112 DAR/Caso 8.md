WLAN

Hardware

If you find a good AP, do let us know. What I would list as a minimum would be:

- PoE support
- Multiple SSID's (for corporate and guests)
- Ceiling mounted
- VLAN hardware support

The idea is to do :

- Radius server
- 802.1X auth
- VLAN per user, depending on login
- Dedicated VLAN for guest SSID
- Simple trunk with 802.1q VLAN's

Software

- OpenWRT | [Soporte](https://toh.openwrt.org/)
- [OpenWisp](https://openwisp.io/docs/dev/index.html) | [Demo](https://openwisp.io/docs/stable/tutorials/demo.html)

NIC inalambrica

OpenNDS Captive Portal