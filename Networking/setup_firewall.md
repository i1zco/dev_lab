# pfSense Firewall

   pfSense Is Firewall in Layer 3 (Network Layer)

**Before we begin any steps!**

we must know that pfSense requires at least two network cards.

- The first card is a WAN card that receives internet or connections from outside the network. 

- The second card is a LAN card for internal devices.

- All of this is so that it acts as an intermediary between connections and traffic in order to control them.


# Dependencies


<p align="center">
  <img src="https://img.shields.io/badge/VirtualBox-212121?style=for-the-badge&logo=virtualbox&logoColor=white" alt="VirtualBox" height="35" />
  <img src="https://img.shields.io/badge/pfSense-212121?style=for-the-badge&logo=pfsense&logoColor=white" alt="pfSense" height="35" />
  <img src="https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows" height="35" />
</p>



## Setup Firewall

**VirtualBox Network Settings**

We will go to the Network Adapters option. You will see a bar at the top with labels such as: Card 1, Card 2

<b>First card:</b> We will make it an external WAN network for the internet.

```Because we are using a virtual environment, we will make it a Bridge Adapter.```

<b>Second Card:</b> We will connect it to the local area network (LAN) as agreed; all of this is to make pfSense the intermediary.


```Again, because we are in a virtual environment, we will choose Internal Network This card will connect to our Windows computer so that we can control the packets entering and leaving it.```

