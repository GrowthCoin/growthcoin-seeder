GROWTHCOIN-SEEDER
=================

growthcoin-seeder is a crawler for the growthcoin network, which exposes a list
of reliable nodes via a built-in DNS server.

Features:
* regularly revisits known nodes to check their availability
* bans nodes after enough failures, or bad behaviour
* accepts nodes down to v1.3.0.1 to request new IP addresses from,
  but only reports good post-v1.3.0.1 nodes.
* keeps statistics over (exponential) windows of 2 hours, 8 hours,
  1 day and 1 week, to base decisions on.
* very low memory (a few tens of megabytes) and cpu requirements.
* crawlers run in parallel (by default 96 threads simultaneously).

REQUIREMENTS
------------

`$ sudo apt-get install build-essential libboost-all-dev libssl-dev`


USAGE
-----

Assuming you want to run a dns seed on _dnsseed.example.com_, you will
need an authorative NS record in _example.com_'s domain record, pointing
to for example _vps.example.com_:

`$ dig -t NS dnsseed.example.com`

```
;; ANSWER SECTION
dnsseed.example.com.   86400    IN      NS     vps.example.com.
```

On the system _vps.example.com_, you can now run `dnsseed`:

`./dnsseed -h dnsseed.example.com -n vps.example.com`

If you want the DNS server to report SOA records, please provide an
e-mail address (with the @ part replaced by .) using -m.

`./dnsseed -h dnsseed.example.com -n vps.example.com -m admin.example.com`

COMPILING
---------

Compiling will require boost and ssl.  On debian systems, these are provided
by `libboost-dev` and `libssl-dev` respectively.

`$ make`

This will produce the `dnsseed` binary.


RUNNING AS NON-ROOT
------------------

Typically, you'll need root privileges to listen to port __53__ (name service).

One solution is using an iptables rule (Linux only) to redirect it to
a non-privileged port:

`$ iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5353`

If properly configured, this will allow you to run `dnsseed` in _userspace_, using
the `-p 5353` option.


CREDITS
-------

__growthcoin-seeder__ was adapted from the original work of [sipa](https://github.com/sipa).
