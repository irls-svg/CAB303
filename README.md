# **Cisco Router & Switch CLI Commands**

- [**Cisco Router & Switch CLI Commands**](#cisco-router--switch-cli-commands)
  - [**Summary**](#summary)
  - [**Commands**](#commands)
    - [Basic Commands](#basic-commands)
    - [Show Commands](#show-commands)
    - [Interface Commands](#interface-commands)
    - [IP Commands](#ip-commands)
    - [VLAN Commands [Switch-only]](#vlan-commands-switch-only)
  - [**Notes, Tips, Terminology, and Common Problems/Solutions**](#notes-tips-terminology-and-common-problemssolutions)
    - [Notes](#notes)
      - [Configuration modes](#configuration-modes)
        - [User EXEC mode](#user-exec-mode)
        - [Privileged EXEC mode](#privileged-exec-mode)
        - [Global configuration mode](#global-configuration-mode)
    - [Tips](#tips)
    - [Common Issues and Questions](#common-issues-and-questions)
      - [What is the difference between `down` and `administratively down`?](#what-is-the-difference-between-down-and-administratively-down)
      - [I keep getting `invalid command`/`command not recognised`/`invalid input detected at '^'`, but I know the command is correct?](#i-keep-getting-invalid-commandcommand-not-recognisedinvalid-input-detected-at--but-i-know-the-command-is-correct)
      - [Not enough ports/correct port type isn't available [Packet Tracer]](#not-enough-portscorrect-port-type-isnt-available-packet-tracer)
    - [Terminology](#terminology)
  - [**Examples**](#examples)
    - [Basic CLI configuration](#basic-cli-configuration)
      - [Getting to the CLI [Packet Tracer]](#getting-to-the-cli-packet-tracer)
        - [For network devices (like routers, switches, firewalls, etc.)](#for-network-devices-like-routers-switches-firewalls-etc)
        - [For host/end devices (like PCs, servers, etc.)](#for-hostend-devices-like-pcs-servers-etc)
      - [Configure router](#configure-router)
        - [General config](#general-config)
        - [Interface config](#interface-config)
        - [Save config](#save-config)
      - [Configure switch](#configure-switch)

## **Summary**

Something I chucked together to explain the basics of using the Cisco CLI (in packet tracer) for CAB303/CAB222, plus some extra stuff I think might be useful -- answers to common questions, common issues, that kinda thing.

Will update as run into things (slash when I get a chance). Hopefully I'll be able to add some more examples that are relevant to the weekly pracs, too. Feel free to LMK if there's anything you'd like added, or any issues.

## **Commands**

**Note:**
Not all commands can be used on both routers and switches, some are router-only and some are switch-only. I've specified the switch-only ones where I've remembered (there's probably some I missed, sorry!), but didn't specify router-only ones as I figure they're reasonably obvious (anything to do with routing, routing protocols, etc).

### Basic Commands

Basic commands to navigate the CLI, show and save configurations, set up a device, etc., or just some common commands for quick reference.

| COMMAND | ABBREVIATION | PURPOSE | EXPLANATION |
|---|---|---|---|
| `?` | | Help | Displays a list of commands |
| `<letter/part of command>?` | | Show available commands | Displays a list of commands that start with the specified letter/portion of command |
| `enable` | `en` | Enter privileged command mode (privileged EXEC) | "Login" to admin panel, essentially |
| `configure terminal` | `conf t` | Enter global configuration mode | Enter global configuration mode to allow changes to device configuration settings - hostname, interfaces, etc. |
| `do` | `do` | Execute a command in privileged mode | Prefix used to run commands like `show`, `ping` whilst in privileged mode (`conf t`), as normally these can only be done in standard mode |
| `hostname` | `ho` | Set the hostname of the device | |
| `show` | `sh` | Show information | Show information about the device, such as `sh ip int br` (`show ip interface brief`) |
| `show running-config` | `sh run` | Display running configuration | Display the current set configuration |
| `show startup-config` | `sh start` | Display startup configuration | Display the saved configuration (different to running-config as you can change config whilst device is in use, but may not necessarily save/need to save the new config) |
| `interface <interface (port)>` | `int` | Enter interface configuration mode | Used to enter configuration for individual interfaces (ports), such as `int Fa0/1` (`interface fastEthernet 0/1`) |
| `no <command/setting>` | `no` | Remove IP address from interface | e.g. after setting ip address to interface incorrectly, remove incorrect one using `no ip add <ip> <netmask>` |
| `exit` | `exit` | Exit current mode | Used to exit current mode, such as exiting interface configuration mode or exiting privileged mode (basically takes you back one step) |
| `end` | | Return to privileged EXEC mode | Used to exit all configuration modes and return to privileged mode |
| `write` | `wr` | Save configuration | Used to save the current configuration to the startup-config |
| `reload` | `reload` | Reload device | Used to reload the device, such as after making changes to the configuration |
| `ping <ip address>` | `ping` | Ping an IP address |  |
| `traceroute <ip address>` | `tracert` | Trace route to an IP address |  |

### Show Commands

These are commands used to show information about different configurations, like interface configuration, routing tables, MAC address tables, so forth. Great for troubleshooting -- `sh ip int br` and `sh ip route` are good friends of mine.

> **Note:** `show` commands will only work in privileged mode (post-`enable`, pre-`conf t`). If you want to run them whilst in configuration mode, you need to prefix them with `do`, e.g. `do sh ip int br`.

| COMMAND | ABBREVIATION | PURPOSE | EXPLANATION |
|---|---|---|---|
| `show ip interface brief`* | `sh ip int br` | Display interfaces IP & netmask configuration | Used to view what IP addresses are assigned to which interface, interface status (down/up/administratively down), subnet mask |
| `show ip route` | `sh ip route` | Display routing table | Used to view the routing table, including the destination network, subnet mask, gateway, interface, and MTU |
| `show mac address-table` | `sh mac add` | Display MAC address table **[Switch-only]**| Used to view the MAC address table, including the VLAN, MAC address, type, interface, and age |
| `show ip protocols` | `sh ip proto` | Display routing protocols | Used to view the routing protocols, including the routing protocol, routing process ID, and routing protocol status |
| `show ip ospf neighbor` | `sh ip ospf neigh` | Display OSPF neighbors | Used to view the OSPF neighbors, including the neighbor ID, interface, priority, state, and DR/BDR |
| `show ip ospf interface` | `sh ip ospf int` | Display OSPF interfaces | Used to view the OSPF interfaces, including the interface, IP address, area, cost, hello interval, dead interval, and state |
| `show vlan brief`* | `sh vlan br` | Display VLAN information **[Switch-only]** | Used to view VLAN info, including the VLAN ID, name, status, and interfaces |

*: `show ip interface brief` and `show vlan brief` both have versions without the `brief` specificer that will display more detailed specific, detailed information, but generally you won't need this -- the `brief` view is more than sufficient unless you're doing super complex stuff (above CAB303/222 level).

### Interface Commands

These are commands used to configure interfaces (ports) on the device, such as setting an IP address, enabling/disabling interfaces, etc.

| COMMAND | ABBREVIATION | PURPOSE | EXPLANATION |
|---|---|---|---|
| `interface <interface (port)>` | `int` | Enter interface configuration mode | Used to enter configuration for individual interfaces (ports), such as `interface fastEthernet 0/1` (`int fa0/1`) |
| `interface range <interfaces>` | `int range` | Enter interface range configuration mode | Used to enter configuration for a range of interfaces, e.g. `interface range fastEthernet 0/1-4` (`int range fa0/1-4`) |
| `interface vlan <vlan number>` | `int vlan` | Enter VLAN interface configuration mode | Used to enter configuration for VLAN interfaces, such as `interface vlan 1` (`int vlan 1`) |
| `shutdown` | `shut` | Shutdown interface | Shutdown an interface that is currently up/online -- make it 'administratively down' |
| `no shutdown` | `no shut` | Bring interface online | Bring an interface that is currently down online -- always needed on inital interface configuration |

### IP Commands

| COMMAND | ABBREVIATION | PURPOSE | EXPLANATION |
|---|---|---|---|
| `ip address <address> <subnet mask>` | `ip add` | Assign IP address to interface | Used to assign an IP address to an interface after entering interface configuration mode |
| `ip default-gateway <ip address>` | `ip def-gw` | Set default gateway | Used to set the default gateway for the interface |
| `ip route <destination network> <subnet mask> <gateway>` | `ip route` | Add static route | Add a static route to the routing table -- gateway can be either an interface or next hop address |
| `no ip domain-lookup` | `no ip domain-lookup` | Disable DNS lookup | |
| `ip name-server <ip address>` | | Set the DNS server | I'm honestly not too sure about this one, haven't really used it much -- will update when I have more info |

### VLAN Commands [Switch-only]

| COMMAND | ABBREVIATION | PURPOSE | EXPLANATION |
|---|---|---|---|
| `vlan <vlan number>` | `vlan` | Enter VLAN configuration mode | Used to enter configuration for a VLAN, such as `vlan 1` |

## **Notes, Tips, Terminology, and Common Problems/Solutions**

### Notes

#### Configuration modes

Summarised from the [Cisco documentation](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/fundamentals/configuration/15mt/fundamentals-15-mt-book/cf-cli-basics.html):

The Cisco command-line interface is divided into different command modes. Each command mode has its own set of commands available for the configuration, maintenance, and monitoring of router and network operations. Commands available to you depend on the mode you are in.

##### User EXEC mode

When you start a session, you generally begin in user EXEC mode -- one of the two EXEC mode access levels. Only a limited subset of EXEC commands are available in user EXEC mode, for security reasons. This level of access is reserved for tasks that do not change the configuration of the router, such as determining the router status.

##### Privileged EXEC mode

To have access to all commands, you must enter privileged EXEC mode -- the second EXEC mode access level. In privileged EXEC mode you can enter any EXEC command, as privileged EXEC mode is a superset of the user EXEC mode commands. Most EXEC mode commands are one-time commands, such as `show` or `more`. Normally, you'd need a password to enter this mode.

##### Global configuration mode

From privileged EXEC mode, you can enter global configuration mode to enter commands that configure general system characteristics, as well as enter specific configuration modes. Configuration modes, including global, allow you to make changes to the running configuration. If you save the configuration, the changes are stored across router reboots.

From this mode you can enter configuration submodes -- these are protocol or feature-specific configuration modes, such as interface configuration mode or VLAN configuration mode.

### Tips

- Command abbreviations are just shortening the command to the smallest it can be whilst still being unique, e.g. `show` can be shortened to `sh` as the letters 'sh' can't expand to anything other than `show` -- `s`, however, could be `send`/`setup`/`show`/`ssh`.
- The CLI generally isn't case-sensitive (in the case of Cisco devices).
- Interface names can be referred to by their full name (e.g. `fastEthernet0/1`) or by their abbreviated name (`f0/1`).
  - You may see variations on `f0/1` (e.g. `Fa0/1`, `fa0/1` or, for serial interfaces, `se0/1`, `s0/1`, `Se0/1`), but they all refer to the same interface. Some abbreviations may not work in all versions of devices, though (not sure), so if `f0/1` doesn't work, try `fa0/1` or `fastEthernet0/1`.
- Use `no ip domain-lookup` to disable DNS lookups -- gets rid of the annoying `Translating "[x]"...domain server (255.255.255.255)` message that appears if you try to do something that doesn't exist (e.g. typing `help` instead of `?`).
- You can enter configuration for multiple interfaces at once using the `interface range` command, e.g. `int range fa0/1-4` will enter configuration for interfaces `fastEthernet0/1`, `fastEthernet0/2`, `fastEthernet0/3`, and `fastEthernet0/4` all at once.
  - Depending on the device, they can be funky about this (especially the spacing). For example, on a Cisco 2960, `int range fa0/1-4` will work, but `int range fa0/1 - 4` won't. On a Cisco 3750, `int range fa0/1 - 4` will work, but `int range fa0/1-4` won't. (based copilot writing that again)
- [Packet Tracer] Hovering over a device for a bit will show a quick summary of the device, including the hostname, IP address, MAC address, and interface information (similar to `show ip int brief`).
- [Packet Tracer] My personal opinion/recommendation is don't touch the speed/play controls unless it's to fast-forward. If you pause and forget to press play again, pressing play on the simulation probably won't be the first thing you think of if something isn't working, you'll start troubleshooting the device/config first rather than packet tracer itself.

### Common Issues and Questions

#### What is the difference between `down` and `administratively down`?

`down` means the interface is physically down, e.g. the cable is unplugged, but administratively up (i.e. configured to be online using `no shutdown` or ticking the 'On' box). Could be issue with physical cable/port, IP settings, misconfiguration -- check physical ports and cables for any issues, double check IP configurations using `show` commands, try `traceroute`/`ping` to identify where the problem is occuring.

Personally, I find this most often occurs due to a cabling issue, so I'd recommended always trying cable checks, port checks, and cable substitution first. Also, check that the destination device is on (:sweat_smile:).

#### I keep getting `invalid command`/`command not recognised`/`invalid input detected at '^'`, but I know the command is correct?

You're probably trying to run a command in the wrong mode, e.g. trying to run `show` in privileged mode. Try again, but chuck a `do` in front of whatever command you're trying to run this time and hopefully you'll have more luck. Otherwise, if it's an interface command, make sure you've got the right interface name for the device you're using (e.g. `f0/0` vs. `s0/0/0` vs. `s0/0`, etc.)

#### Not enough ports/correct port type isn't available [Packet Tracer]

This is funky as hell, but go to the 'Physical' config tab on the device window, click on the power button to turn the device off (may need to zoom in), then you can click and drag different modules from the panel on the left-hand side into empty slots on the device. Make sure to turn the device back on once you're done.

### Terminology

[Major WIP]

| TERM | MEANING | MORE INFO/RESOLUTION |
|--|--|--|
| user EXEC mode | Basic user mode, can only run a few commands -- use `en` to get into the good stuff. | Prompt will look like `Hostname>`. From Cisco docs: "When you start a session on a router, you generally begin in user EXEC mode , which is one of two access levels of the EXEC mode. For security purposes, only a limited subset of EXEC commands are available in user EXEC mode. This level of access is reserved for tasks that do not change the configuration of the router, such as determining the router status." |
| privileged EXEC mode | The good stuff, can run more commands like `show`, `ping`, `conf t`, etc. | Prompt will look like `Hostname#` |
| global configuration mode | Used to configure the device itself, such as hostname, DNS, etc. | Prompt will look like `Hostname(config)#` |
| interface configuration mode | Used to configure interfaces, such as IP addresses, descriptions, etc. | Prompt will look like `Hostname(config-if)#` |
| down | Interface is administratively up (i.e. online) but physically down. Could be issue with physical cable/port, IP settings, misconfiguration | Check physical ports and cables for any issues, double check IP configurations using `show` commands, try `traceroute`/`ping` to identify where the problem is occuring[^1] |
| administratively down | Interface is administratively shutdown, i.e. offline -- purposely shutdown/default state | Enter interface config mode and use `no shutdown` to bring interface online |
| invalid command/command not recognised | You're probably trying to run a command in the wrong mode, e.g. trying to run `show` in privileged mode | Try again, but chuck a `do` in front of whatever command you're trying to run this time |

## **Examples**

### Basic CLI configuration

This will be a very basic example, and is definitely not intended to be a complete guide to setting up a network. It's just a show of how to get started with the basics.
(big thank you to my beloved sponsor, github copilot, for writing this blurb)

> **Note:** Most of the time I'll be using the abbreviated form of the commands because I'm lazy and they save time, but I'll make sure to include the full command in the commands list above.

#### Getting to the CLI [Packet Tracer]

##### For network devices (like routers, switches, firewalls, etc.)

- Click on the device you want to configure, then hit the 'CLI' tab up the top of the device window.

##### For host/end devices (like PCs, servers, etc.)

This will get you into what is essentially a Windows command prompt -- can use `ping`, `tracert`, `ipconfig /all`, etc. to troubleshoot.

- Click on the device whose CLI you want to access, select the 'Desktop' tab from the top of the device window, then click on the 'Command Prompt' option.

#### Configure router

##### General config

> On boot, you'll probably be prompted with `Would you like to enter the initial configuration dialog? [yes/no]:` -- **enter `n` and press return/enter** to skip!

1. Skip the initial config dialog by entering `n` and hitting return/enter.
   - If you do happen to enter it, just hit `Ctrl+C` to exit.
   - We skip this because we'll be configuring the device manually, and it takes you through a bunch of extra, unnecessary (for this unit) options.
2. Enter privileged EXEC mode [`enable`/`en`]
   - May need to enter the 'enable' password if set/prompted (default is `cisco`).
   - This is where you can run `show`/`ping`/`traceroute` etc. commands without having to put `do` in front.
3. Enter global configuration mode [`conf t`]
4. Set hostname (if desired, would recommend) [`hostname <hostname>`]
5. Disable DNS lookups [`no ip domain-lookup`]
   - Only do this if you **DON'T** need a DNS server configured on the *router*.
     - I haven't looked at all the CAB303/222 pracs in detail, but I don't think this is something you'll need to do in the labs (and up to Week 6, definitely not) -- you should be safe to disable it, no worries.
6. Exit global configuration mode [`exit`]

##### Interface config

(continues from 5. above)

1. From privileged EXEC mode, enter interface (i.e. port) configuration mode by entering the name of the interface you want to configure [`int <interface name>`]
   - `<interface>` can be the full name (e.g. `fastEthernet0/1`) or the abbreviated name (e.g. `f0/1`, `fa0/1`).
   - In this example I'll use `s2/0`, so the command would be `int s2/0`.
2. Set an IP address on the interface [`ip address <ip address> <subnet mask>`]
   - For this example, I'll use `ip address 200.0.0.1 255.255.255.0`.
3. Set the interface to be online [`no shutdown`]
   - Equivalent to ticking the 'On' box in the GUI.
4. Configure another interface (if desired) by repeating steps 1-3
   - Can switch directly between interfaces using `int <interface name>` without having to exit the mode.
   - I'll do an Ethernet port now, so
     1. `int f0/1`
     2. `ip address 192.168.100.33 255.255.255.224`
     3. `no shutdown`
5. Exit interface configuration mode once done [`exit` or `end`]

##### Save config

> **Note:** This is **very** important! If you don't save the running config, it'll be lost when you restart the device and you'll have to do it all again :(

1. Verify current running config is correct [`show run`]
   - This is assuming you're in privileged EXEC mode (no `(config)` in prompt). If you're still in config mode, you'll enter `do show run` instead.
2. To save the running config to the startup config, enter [`wr`]
   - Older tutorials may use `copy running-config startup-config`, but this is now deprecated.
3. Verify the running config has been saved by viewing startup config to ensure they match [`sh start`]

#### Configure switch

TBA -- but it's pretty much the same as the router for the real basic stuff.
