
.. Secure Peer Networking with TINC slides file, created by
   hieroglyph-quickstart on Sun Nov 15 21:40:13 2015.


================================
Secure Peer Networking with TINC
================================

.. figure:: _static/14x_logo_lg.png
   :align: left
   :width: 300px

Spencer Krum

Jan 23rd, 2016

IBM

@nibalizer


.. note::

   * Who am I
   * What do I work on
   * github


Portland
========

.. figure:: _static/mt_hood.jpg
   :align: center


Logos
=====


.. figure:: _static/tinc-logo.jpg
   :align: left
   :width: 300px

.. figure:: _static/consul-logo.jpg
   :align: right
   :width: 300px


.. note::

   * I've wanted to give this talk for a long time
   * This is a talk about tinc
   * A little about consul


Tinc is a VPN
=============

.. figure:: _static/A-VPN-is-best-for-maintaining-online-security.jpg
   :align: center


.. note::
    * Mesh VPN
    * Different than point-to-point VPNs like OpenVPN



Tinc Fast Facts
===============


.. figure:: _static/year-2000-nokia.jpg
   :align: center


.. note::

    * Written in C
    * Started in 1998, first commited to src in 2000
    * Portable (\*nix, Windows, Android, IOS incoming)
    * Daemon
    * 99% 2 Developers
    * freenode channel / mailing list / dev process
    * 15 % link speed slowdown in the cloud
    * Port 655 in /etc/services


The Network
===========


.. figure:: _static/the-network-is-the-computer.jpg
   :align: center

.. note::

    * Use this with my friends /  not prod lol
    * Bringing back the network / https as the only service sucks
    * University Network
    * LAN Parties
    * Services
    * Blurry line between workstation and server
    * we run it on laptops and home routers and the occasional rackmount gear



VPN & Network
=============

.. figure:: _static/tinc_dot.jpg
   :align: center


.. note::


  * this pic Generated every minute
  * Flat IP space
  * Daemon = node
  * Each daemon responsible for a subnet and an ip addr
  * Continually probes for most efficient routes
  * Re-routes around failures


VPN & Network
=============

.. figure:: _static/tinc_consulstart_network1.jpg
   :align: center

.. note::

   * tinc has a concept of 'connect to'
   * Connections don't have to be reflexive
   * Network trafic is bidirectional regardless
   * These nodes are laptops or servers or home routers
   * basically comes down to which nodes have a known public ip
   * public/private keys


Getting Status
==============


.. code-block:: bash
   :emphasize-lines: 5,9

   kill -USR2 $(pidof tincd); tail /var/log/syslog

   Edges:
     bkero to spencer at 131.xxx.xx.xx  weight 1538
     spencer to bkero at 216.xxx.xx.xx  weight 1538
   End of edges.
   Subnet list:
     10.11.11.128/25#10 owner spencer
     10.11.22.0/24#10 owner bkero
   End of subnet list.


.. note::

   * tinc uses signals to communicate
   * dumps to syslog by default
   * ALRM, USR1, USR2, HUP, INT


Getting Status (Improved)
=========================


.. code-block:: bash

   curl -s -i http://127.0.0.1:9000/tincstat
   {
     "total_bytes_in": 115324,
     "total_bytes_out": 67990,
     "connections": [
       {
         "name": "bkero",
         "ip": "216.xx.xx.xx",
         "port": 4545
       }
     ]
   }


https://github.com/nibalizer/tincstat


.. note::
   * go utility
   * run as a daemon, partialy parses the log output
   * the motivation for me was to put it into my statusbar on my computer
   * 1.1 will bring a tinc info command, control socket


Now What
========


.. figure:: _static/malcom.jpg
   :align: center


Services
========

* A few things can just be turned on immediately

 * Apache
 * UPnP
 * VLC Streaming
 * StarCraft


A Problem Arises
================


* DNS


.. note::

  * You think its dns at first, and we did
  * Solved it the way we thought we should, with hosts files
  * Briefly ran a bind server, that didn't scale
  * The problem is there isn't one admin domain, there are many
  * Even with domains solved, how would we say what protocols?
  * The need is for something mutable and highly available


The Requirements
================

Something mutable and highly available


.. note::
  * mutable because many people need to modify it
  * highly available because nodes die all the time


Let's do something Hip
======================



.. figure:: _static/Etcd.png
   :align: center


.. note::
  * etcd is software from coreos
  * originally designed to store configs for docker because docker is write
  * sometimes refered to as a 'distributed lock manager'
  * raft consensus protocol
  * hierarchal key-value store
  * highly available, can be configured for n+2
  * start writing hostname -> ip mappings in it
  * working on a script to dump etcd keys and output a hosts file or something


Let's do something stupid
=========================


.. figure:: _static/dangerous-forklift.jpg
   :align: center

.. note::
  * how many people know what libnss is
  * name service switcher
  * turns out you can write endpoints for the name service switcher
  * in c
  * someone writes a libnss-etcd, which basically just shells out to the etcdctl utility
  * dns is solved!


Let's do the hippest thing imaginable
=====================================


.. figure:: _static/consul-logo.jpg
   :align: center


.. note::
  * consul was going to come back
  * turns out the janky c code to get in the way of dns lookups, that was build into consul
  * consul can respond for keys inside dns
  * consul can also do nagios-like healthchecks, to evaluate which services have died and which have not
  * these are hackers so services are going up and going down all the time


Demo
====


Neat Tricks
===========


* Laptops and other "behind nat" devices have permanent ip addrs
* Backup Device
* Any daemon can ping your laptop, laptop can run services
* BroodWar over tinc
* SSH doesn't timeout
* Transpacific


NFS
===

* NFS + AutoFS works great on tinc
* Read-Only mounts mostly
* Could even do nfs-homedir for a laptop user



X11
===

* Designed to be run over a network
* Can listen on a TCP socket
* Ever wonder what DISPLAY=:0 was actually doing?

** Can set DISPLAY=192.168.1.100:0 to run over a network
** Useful combined with xpra (screen for X)


What's Next
===========

* Indexing
* Tinc 1.1
* Development


Conclusions
===========

* Tinc can be used to build an overlay network
* Direct application of that to a real problem is hard
* Consul and Etcd are robust, but obtuse to work with as a human
* StarCraft is an excellent game


Thank You
=========

.. figure:: _static/spencer_face.jpg
   :align: left

Spencer Krum

IBM

@nibalizer

nibz@spencerkrum.com

https://github.com/nibalizer/tinc-presentation



