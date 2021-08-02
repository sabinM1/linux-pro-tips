# linux-pro-tips
### When you had enough of troubleshooting

I. Specific DE/distro/hardware etc. problems:
  1. [GNOME](https://github.com/sabinM1/linux-pro-tips/blob/master/GNOME.md)

II. General:
  1. [Wake-on-LAN with NetworkManager](#wake-on-lan-with-networkmanager)

III. Notes:
  1. **Bold** sections should be considered important.
  2. This project was made to remind myself to never fix a working machine.
  3. Pull Requests are welcome.
  4. The commands/information found in this project are provided without any kind of warranty.
  5. In case of redistribution, be sure to keep and respect the [GPL-3.0 License](https://github.com/sabinM1/linux-pro-tips/blob/master/LICENSE).

# Wake-on-LAN with NetworkManager
##### From [wiki.archlinux.org](https://wiki.archlinux.org/title/Wake-on-LAN#NetworkManager)

NetworkManager provides [Wake-on-LAN ethernet support](https://www.phoronix.com/scan.php?page=news_item&px=NetworkManager-WoL-Control). One way to enable Wake-on-LAN by magic packet is through ''nmcli''.

First, search for the name of the wired connection:

`nmcli con show`:
```
NAME    UUID                                  TYPE            DEVICE
wired1  612e300a-c047-4adb-91e2-12ea7bfe214e  802-3-ethernet  enp0s25
```

By following, one can view current status of Wake-on-LAN settings:

`nmcli c show "wired1" | grep 802-3-ethernet.wake-on-lan`:
```
802-3-ethernet.wake-on-lan:             default
802-3-ethernet.wake-on-lan-password:    --
```

**Enable Wake-on-LAN by magic packet on that connection:**

 `nmcli c modify "wired1" 802-3-ethernet.wake-on-lan magic`

**Then reboot, possibly two times. To disable Wake-on-Lan, substitute `magic` with `ignore`.**

The Wake-on-LAN settings can also be changed from the GUI using [nm-connection-editor](https://archlinux.org/packages/?name=nm-connection-editor).

You can disable Wake-on-Lan for all connections permanently by adding a dedicated configuration file:

`/etc/NetworkManager/conf.d/''wake-on-lan.conf''`:
```
[connection]
ethernet.wake-on-lan = ignore
wifi.wake-on-wlan = ignore
```

=== Enable WoL in TLP ===

**When using [TLP](https://wiki.archlinux.org/title/TLP) for suspend/hibernate, the `WOL_DISABLE` setting should be set to `N` in `/etc/tlp.conf` to allow resuming the computer with WoL.**
