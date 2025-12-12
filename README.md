## Install various plugins (AdGuardHome, Blocky, ControlD, NextDNS, Speedtest, Telegram) on OPNsense

### Updated for OPNsense v25.7

---
INSTALL instructions (LATEST)

1. Login to OPNsense shell.
2. Execute the below two commands in OPNsense shell (either as root user or use sudo)

```
sudo fetch -o /usr/local/etc/pkg/repos/guru-repo.conf https://gspannu.github.io/OPN-Plugins/repo/guru-repo.conf
sudo pkg update -f
```

3. On OPNsense, login to GUI. Go to System> Firmware> Plugins, Enable Show Community Plugins
4. Search for guru, it will list all plugins like AdGuardHome, Blocky, etc...
4. Install the required plugin
5. Refresh the browser page to reload OPNsense GUI

---

Update: 2025-12-12
* Repo packages now hosted on Github pages due to some issues with Last Modified Header
* Updated .conf file to new location on Github pages

* Use new conf file, in OPNsense shell
```
sudo fetch -o /usr/local/etc/pkg/repos/guru-repo.conf https://gspannu.github.io/OPN-Plugins/repo/guru-repo.conf
sudo pkg update -f
```

---
Update: 2025-12-11
* Updated Blocky to v0.28.2
* Updated AdGuardHome to v0.108.0-b.80 (beta version)
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins
  for OPNsense stable 25.7 release

---
Update: 2025-11-13(1)
* Updated Blocky to v0.28.1
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins
  for OPNsense stable 25.7 release

---
Update: 2025-11-13
* Updated Blocky to v0.28.0
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins for OPNsense stable 25.7 release

---
Update: 2025-11-11
* Updated Blocky to v0.27.0
* Updated AdGuardHome to v0.108.0-b.78 (beta version)
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins for OPNsense stable 25.7 release

---
Update: 2025-08-08
* Updated AdGuardHome to v0.108.0-b.73
* Updated ControlD to 1.4.5
* Updated NextDNS to 1.46
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins for OPNsense stable 25.7 release

---
Update: 2025-07-11
* Updated Blocky to version 0.26.2
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins for OPNsense stable 25.1 release

---
Update: 2025-04-16
* Added Telegram (Monit alerts to Telegram bot) plugin to this repo
* Contains AdGuardHome, Blocky, ControlD, NextDNS, Speedtest and Telegram plugins for OPNsense stable 25.1 release

---
Update: 2025-02-19
* Added Speedtest Plugin (with custom widget) to this repo
	Copy of speedtest plugin from mimugmail, but added custom widget
* Removed support for FreeBSD v13
* Updated AdGuardHome beta version
* Contains AdGuardHome, Blocky, ControlD, NextDNS and Speedtest plugins for OPNsense stable 25.1 release


Update: 2025-02-01
* Updated AdGuardHome,Blocky,NextDNS plugins for OPNsense stable 25.1 release

Update: 2025-01-20
* Updated Blocky to v0.25 and for OPNsense v25.1b
* Updated NextDNS for OPNsense v25.1b

Update: 2025-01-03
* Updated AdGuardHome DNS package

--------------------------------
Update: 2024-10-26
* Added 2 more packages to the repo (**ControlD** and **NextDNS**)
* Both operate the same as Blocky and are other DNS Resolvers with AdBlocking
* Same rules as Blocky (see below) apply...

--------------------------------
1. Login to OPNsense shell.
2. Execute the below two commands in OPNsense shell (either as root user or use sudo)

```
sudo fetch -o /usr/local/etc/pkg/repos/guru-repo.conf https://raw.githubusercontent.com/gspannu/OPN-Plugins/master/guru-repo.conf;
sudo pkg update -f
```

2. On OPNsense, login to GUI. Go to System> Firmware> Plugins and search for os-blocky
3. Install os-blocky
4. Refresh the browser page to reload OPNsense GUI

5. If Blocky is being used as the 'Primary DNS' then remember to _**disable**_ Unbound/dnsmasq or setUnbound/dnsmasq _**listening port**_ to something other than _**53**_, else Blocky DNS will not execute and throw an error.
6. Goto Services, and a new entry for Blocky DNS should be available. Enable/ use as appropriate.
7. Once Blocky is confirmed working, you may configure the Blocky DNS configuration by directly editing blocky-config.yml file in /usr/local/etc/blocky/blocky-config.yml. Use something like `nano /usr/local/etc/blocky/blocky-config.yml`
8. On OPNsense GUI > Disable and Renable Blocky DNS
9. All errors are logged in '/var/log/blocky/blocky.log'
10. Also check that Blocky DNS is working as the **primary dns** resolver.
In OPNsense shell, execute `cat /etc/resolv.conf` and it should have a nameserver entry for localhost (e.g. _nameserver 127.0.0.1_).
```
[OPNsense] router:~ $ cat /etc/resolv.conf 
domain home
nameserver 127.0.0.1
search home
```
>OPNsense may require a restart for this to work. It is advisable to restart OPNsense and check this.

11. OPNsense made some changes recently where the primary DNS (:53) now needs to be advertised specifically by 3rd party DNS resolvers, like Blocky, AdGuardHome, etc. The plugin has been updated with this property and it shoudl work. However, it is a good idea to define the DNS server (for each affected interface) in the DHCP page of OPNsense. See below.
>OPNsense GUI > Services > ISC DHCPv4 > [InterfaceName] > DNS servers. Specify the [interface IP address] address here e.g. 192.168.1.1 or 192.168.10.1. Do this for all DHCPv4 interfaces and it ensures that all clients will obtain the router IP address as DNS resolver (just in case Blocky fails to register on port 53). Read about this issue here for more details: https://github.com/opnsense/core/issues/6513#issuecomment-1516642071
---

12. **Removing os-blocky plugin**
- Disable Blocky DNS from OPNsense GUI > Services > Blocky DNS
- Ensure that internet is working fine (i.e. enable some other DNS resolver, etc.)
- Delete Blocky from OPNsense
  - GUI > System > Firmware > Plugins : Remove os-blocky
- Refresh OPNsense GUI browser

- Now login to OPNsense shell
```
sudo rm /usr/local/etc/pkg/repos/guru-repo.conf
sudo pkg update -f
```

---
### Happy DNS resolving and Adblocking !
