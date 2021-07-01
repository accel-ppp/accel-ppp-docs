BRAS tuning
===========

Recommendations for BRAS (Broadband Remote Access Server) performance.


Network tuning
--------------

Disable kernel mitigations to maximize performance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Edit file ``/etc/default/grub``

.. code-block:: sh

  GRUB_CMDLINE_LINUX_DEFAULT="crashkernel=128M intel_idle.max_cstate=0 processor.max_cstate=1 idle=poll ixgbe.allow_unsupportd_sfp=1 quiet consoleblank=0 biosdevname=1 audit=0 noibrs noibpb nopti nospectre_v2 nospectre_v1 l1tf=off nospec_store_bypass_disable no_stf_barrier mds=off mitigations=off pcie_aspm=off"


After saving, please update grub settings 

.. code-block:: sh

  $sudo update-grub

Warning! Enabling the idle loop  (``idle=poll``) parameter can cause 100% CPU utilization on your VM (if you're using virtual enviroments like ProxMox)


Disable NIC offloads
^^^^^^^^^^^^^^^^^^^^
Disable hardware offloads,increase Tx/Rx buffers and queue length on your NICs to prevent speed problems.

Debian ``/etc/network/interfaces``:


.. code-block:: sh
  
    allow-hotplug eth0
    iface eth0 inet manual
        up ethtool -K eth0 rx off tx off tso off gso off gro off lro off rxvlan off txvlan off rx-vlan-filter off sg off rxhash off &> /dev/null
        up ethtool -K eth0 tso off gso off gro off ntuple on &> /dev/null
        up ethtool -G eth0 rx 4096 tx 4096 &> /dev/null
        up ifconfig eth0 txqueuelen 10000 &> /dev/null

Please determine your NIC queue and buffers limit before increase:

.. code-block:: sh


  ethtool -g eth0


Fix Download speed problem (shaper)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Change shaper from htb to tbf

.. code-block:: sh

  [shaper]
  …
  down-limiter=tbf 


Default rate limits (shaper)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If radius-server Access-Accept answer has no compatible speed attributes - to prevent unlimited session speed you can add default rate-limits (in Kbps).

``nano /etc/accel-ppp.conf``

.. code-block:: sh

  [shaper]
  rate-limit=888/888


Change PPPoE MTU
^^^^^^^^^^^^^^^^

You can adjust allowed PPPoE min/max MTU/MRU settings:
``nano /etc/accel-ppp.conf``

.. code-block:: sh

  [ppp]
  verbose=1
  min-mtu=1280
  mtu=1492
  mru=1492



Hotplug optimization
^^^^^^^^^^^^^^^^^^^^
To generate hotplug events on IPoE interfaces (Debian 10):

``nano /lib/udev/ifupdown-hotplug``




.. code-block:: sh

    case "$ACTION" in
    add)
    # these interfaces generate hotplug events *after* they are brought up
    case $INTERFACE in
        ppp*|ippp*|isdn*|plip*|lo|irda*|ipsec*



just add ``|ipoe*`` after ``|ipsec*``

