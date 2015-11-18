
.. Secure Peer Networking with TINC slides file, created by
   hieroglyph-quickstart on Sun Nov 15 21:40:13 2015.


Secure Peer Networking with TINC
================================

Contents:

.. toctree::
   :maxdepth: 2


What is TINC?
=============

* Mesh VPN
* Different than point-to-point VPNs like OpenVPN
* Mature codebase, written in C
* Portable (\*nix, Windows, Android, IOS incoming)

VPN & Network
=============

* TODO (Diagram of architecture)
* Flat IP space
* Daemon = node
* Each daemon responsible for a subnet
* Edges shared between nodes on connect
* Continually probes for most efficient routes

Tincd
=====

* Very similar to other services
* Config files in /etc/tinc/$NET
* First run, generate pubkey/privkey: tincd -K -n $NET
* tincd -n $NET --no-detach
* Fun with Signals!

Features
========

* Security (auto-rekeying)
* Convenience
  * New host discovery
  * Auto-rekeying
  * Autoreconnect after network blip
  * Packet buffering means reconnects are transparent
  * This means SSH connections don't DC when laptop sleeps
  * Laptop has permanent IP address

Installation - Install the package
==================================

* TODO (Diagram of distro logos)
* Install the package (tinc)

Installation - Make config files
================================

* mkdir /etc/tinc/$NETWORK for config files
* mkdir /etc/tinc/$NETWORK/hosts to list hosts
* edit /etc/tinc/$NETWORK/tinc.conf
* (Code-block for tinc.conf)

.. code-block:: bash
   :emphasize-lines 2

   $ cat /etc/tinc/examplenet/tinc.conf
   Name = laptop
   AddressFamily = ipv4

Installation - Connect to Node (Optional)
=========================================

* Add a host to connect to (codeblock for host config file)

Is it working?
==============

* Start up tincd with --no-detach
* Signals!
  * SIGHUP - Reread config file, open/close connections
  * ALRM - Reconnect to all hosts
  * INT (^c) - Increase debug level to 5, again to revert
  * USR1 - Prints connection list
  * USR2 - Prints all the info
* Tincstat (github.com/nibalizer/tincstat)

Security
========

* TODO (Picture of big padlock)
* TODO (Add more content)
* Transitive trust - everybody trusts everybody

What now?
=========

* TODO (Screenshot of ping window showing responses)

DNS
===

* TODO (Screenshot of BIND file)

Service Autodiscovery
=====================

* We tried Etcd
  * A user wrote libnss-etcd for hostname resolution
* Eventually switched to Consul
  * Cluster runs across Tinc VPN
  * Handles disconnect/reconnects much better than Etcd

What to store in our HA key-value DB?
=====================================

* DNS information
* Autofs + NFS
* Host information

NFS
===

X11
===

* Designed to be run over a network
* Can listen on a TCP socket
* Ever wonder what DISPLAY=:0 was actually doing?
  * Can set DISPLAY=192.168.1.100:0 to run over a network
  * Useful combined with xpra (screen for X)
