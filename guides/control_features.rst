Control features
================

Accel-ppp support next features for control daemon and sessions:

	* ``accel-cmd``

	* ``telnet``
	
	* ``snmp``
	
	* ``RADIUS COA``
	
Common available commands for ``accel-cmd`` and ``telnet``. Also possible show this help message with one of commands  ``accel-cmd help`` and ``telnet 127.0.0.1  2000`` then run ``help``.

.. code-block:: sh

	show stat - shows various statistics information
	terminate if <interface> [soft|hard]- terminate session by interface name
		[match] username <username> [soft|hard]- terminate session by username
		ip <addresss> [soft|hard]- terminate session by ip address
		csid <id> [soft|hard]- terminate session by calling station id
		sid <id> [soft|hard]- terminate session by session id
		all [soft|hard]- terminate all sessions
	reload - reload config file
	restart [hard] - restart daemon
			hard - restart immediatly
			default action - terminate all connections then restart
	shutdown [soft|hard|cancel]- shutdown daemon
			default action - send termination signals to all clients and wait everybody disconnects
			soft - wait until all clients disconnects, don't accept new connections
			hard - shutdown now, don't wait anything
			cancel - cancel 'shutdown soft' and return to normal operation
	exit - exit cli
	show sessions [columns] [order <column>] [match <column> <regexp>] - shows sessions
		columns:
			netns - network namespace name
			ifname - interface name
			username - user name
			ip - IP address
			ip6 - IPv6 address
			ip6-dp - IPv6 delegated prefix
			type - VPN type
			state - state of session
			uptime - uptime (human readable)
			uptime-raw - uptime (in seconds)
			calling-sid - calling station id
			called-sid - called station id
			sid - session id
			comp - compression/ecnryption method
			rx-bytes - received bytes (human readable)
			tx-bytes - transmitted bytes (human readable)
			rx-bytes-raw - received bytes
			tx-bytes-raw - transmitted bytes
			rx-pkts - received packets
			tx-pkts - transmitted packets
			ipoe-type - IPoE session type
			rate-limit - rate limit down-stream/up-stream (Kbit)
	pppoe mac-filter reload - reload mac-filter file
	pppoe mac-filter add <address> - add address to mac-filter list
	pppoe mac-filter del <address> - delete address from mac-filter list
	pppoe mac-filter show - show current mac-filter list
	pppoe interface add <name> - start pppoe server on specified interface
	pppoe interface del <name> - stop pppoe server on specified interface and drop his connections
	pppoe interface show - show interfaces on which pppoe server started
	pppoe set verbose <n> - set verbosity of pppoe logging
	pppoe set PADO-delay <delay[,delay1:count1[,delay2:count2[,...]]]> - set PADO delays (ms)
	pppoe set Service-Name <name> - set Service-Name to respond
	pppoe set Service-Name * - respond with client's Service-Name
	pppoe set AC-Name <name> - set AC-Name tag value
	pppoe show verbose - show current verbose value
	pppoe show PADO-delay - show current PADO delay value
	pppoe show Service-Name - show current Service-Name value
	pppoe show AC-Name - show current AC-Name tag value
	shaper change <interface> <value> [temp] - change shaper on specified interface, if temp is set then previous settings may be restored later by 'shaper restore'
	shaper change all <value> [temp] - change shaper on all interfaces, if temp is set also new interfaces will have specified shaper value
	shaper restore <interface> - restores shaper settings on specified interface made by 'shaper change' command with 'temp' flag
	shaper restore all - restores shaper settings on all interfaces made by 'shaper change' command with 'temp' flag

accel-cmd
^^^^^^^^^

This application is very powerful and often used if you have `cli` connection. Be default accel-ppp listen *TCP* port *2000*  for input/output with accel-cmd. However `telnet` has same functions, but `accel-cmd` is more comfortable, allow send command without enter in to another environment. Detail about cli you may read at :ref:`cli_configuration` .Let's revise `accel-cmd` possible commands.

  * `accel-cmd show stat` - one of more important command, allow display  *accel-ppp* daemon statistics and information about connections types and something counters such as RADIUS auth, acct summary and lost queries. Detail below:

telnet
^^^^^^^^^


radius CoA
^^^^^^^^^^

Example, terminate session by username: ``echo User-Name=username | radclient -x 127.0.0.1:3799 disconnect testing123``.

snmp
^^^^
