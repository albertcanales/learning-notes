[How to Homelab - Learn Linux TV](https://www.youtube.com/c/LearnLinuxtv/search?query=How%20to%20Homelab)

> Many ideas from this series were already familiar to me, so only content from the Network and Laptop video is noted down.

## Networks

Use *static lease* instead of static IPs:

1. Shrink DHCP range (removing IPs from the pool) to make space for services.
2. Enable "manual assignment" on the configuration
3. When the service host connects, manually change its IP for one outside the pool.

Using **pfsense** as router software. Many capabilities, like the ability to create *VLANs*, dividing the network like so (for example):

- 192.168.1.0/24 for family members
- 192.168.2.0/24 for guests
- ... and so on

## Laptops

Laptop clustering may be easier that expected. The video uses *Proxmox*, a virtualisation software that has the following pros and cons:

- Easily configurable by GUI, includes **individual monitorization** for each VM/Container
- Requires an extra machine running *Proxmox*


