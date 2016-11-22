% firehol-services(5) FireHOL Reference | VERSION
% FireHOL Team
% Built DATE

# NAME

firehol-services - FireHOL services list

# SYNOPSIS


[AH][keyword-service-AH]
[all][keyword-service-all]
[amanda][keyword-service-amanda]
[any][keyword-service-any]
[anystateless][keyword-service-anystateless]
[apcupsd][keyword-service-apcupsd]
[apcupsdnis][keyword-service-apcupsdnis]
[aptproxy][keyword-service-aptproxy]
[asterisk][keyword-service-asterisk]

[cups][keyword-service-cups]
[custom][keyword-service-custom]
[cvspserver][keyword-service-cvspserver]

[darkstat][keyword-service-darkstat]
[daytime][keyword-service-daytime]
[dcc][keyword-service-dcc]
[dcpp][keyword-service-dcpp]
[dhcp][keyword-service-dhcp]
[dhcprelay][keyword-service-dhcprelay]
[dhcpv6][keyword-service-dhcpv6]
[dict][keyword-service-dict]
[distcc][keyword-service-distcc]
[dns][keyword-service-dns]

[echo][keyword-service-echo]
[emule][keyword-service-emule]
[eserver][keyword-service-eserver]
[ESP][keyword-service-ESP]

[finger][keyword-service-finger]
[ftp][keyword-service-ftp]

[gift][keyword-service-gift]
[giftui][keyword-service-giftui]
[gkrellmd][keyword-service-gkrellmd]
[GRE][keyword-service-GRE]

[h323][keyword-service-h323]
[heartbeat][keyword-service-heartbeat]
[http][keyword-service-http]
[httpalt][keyword-service-httpalt]
[https][keyword-service-https]
[hylafax][keyword-service-hylafax]

[iax][keyword-service-iax]
[iax2][keyword-service-iax2]
[ICMP][keyword-service-ICMP]
[icmp][keyword-service-icmp]
[ICMPV6][keyword-service-ICMPV6]
[icmpv6][keyword-service-icmpv6]
[icp][keyword-service-icp]
[ident][keyword-service-ident]
[imap][keyword-service-imap]
[imaps][keyword-service-imaps]
[ipsecnatt][keyword-service-ipsecnatt]
[ipv6error][keyword-service-ipv6error]
[ipv6mld][keyword-service-ipv6mld]
[ipv6neigh][keyword-service-ipv6neigh]
[ipv6router][keyword-service-ipv6router]
[irc][keyword-service-irc]
[isakmp][keyword-service-isakmp]

[jabber][keyword-service-jabber]
[jabberd][keyword-service-jabberd]

[l2tp][keyword-service-l2tp]
[ldap][keyword-service-ldap]
[ldaps][keyword-service-ldaps]
[lpd][keyword-service-lpd]

[microsoft_ds][keyword-service-microsoft_ds]
[mms][keyword-service-mms]
[msn][keyword-service-msn]
[msnp][keyword-service-msnp]
[ms_ds][keyword-service-ms_ds]
[multicast][keyword-service-multicast]
[mysql][keyword-service-mysql]

[netbackup][keyword-service-netbackup]
[netbios_dgm][keyword-service-netbios_dgm]
[netbios_ns][keyword-service-netbios_ns]
[netbios_ssn][keyword-service-netbios_ssn]
[nfs][keyword-service-nfs]
[nis][keyword-service-nis]
[nntp][keyword-service-nntp]
[nntps][keyword-service-nntps]
[nrpe][keyword-service-nrpe]
[ntp][keyword-service-ntp]
[nut][keyword-service-nut]
[nxserver][keyword-service-nxserver]

[openvpn][keyword-service-openvpn]
[oracle][keyword-service-oracle]
[OSPF][keyword-service-OSPF]

[ping][keyword-service-ping]
[pop3][keyword-service-pop3]
[pop3s][keyword-service-pop3s]
[portmap][keyword-service-portmap]
[postgres][keyword-service-postgres]
[pptp][keyword-service-pptp]
[privoxy][keyword-service-privoxy]

[radius][keyword-service-radius]
[radiusold][keyword-service-radiusold]
[radiusoldproxy][keyword-service-radiusoldproxy]
[radiusproxy][keyword-service-radiusproxy]
[rdp][keyword-service-rdp]
[rndc][keyword-service-rndc]
[rsync][keyword-service-rsync]
[rtp][keyword-service-rtp]

[samba][keyword-service-samba]
[sane][keyword-service-sane]
[sip][keyword-service-sip]
[smtp][keyword-service-smtp]
[smtps][keyword-service-smtps]
[snmp][keyword-service-snmp]
[snmptrap][keyword-service-snmptrap]
[socks][keyword-service-socks]
[squid][keyword-service-squid]
[ssh][keyword-service-ssh]
[stun][keyword-service-stun]
[submission][keyword-service-submission]
[sunrpc][keyword-service-sunrpc]
[swat][keyword-service-swat]
[syslog][keyword-service-syslog]

[telnet][keyword-service-telnet]
[tftp][keyword-service-tftp]
[time][keyword-service-time]
[timestamp][keyword-service-timestamp]
[tomcat][keyword-service-tomcat]

[upnp][keyword-service-upnp]
[uucp][keyword-service-uucp]

[vmware][keyword-service-vmware]
[vmwareauth][keyword-service-vmwareauth]
[vmwareweb][keyword-service-vmwareweb]
[vnc][keyword-service-vnc]

[webcache][keyword-service-webcache]
[webmin][keyword-service-webmin]
[whois][keyword-service-whois]

[xbox][keyword-service-xbox]
[xdmcp][keyword-service-xdmcp]

# DESCRIPTION


## service: AH

IPSec Authentication Header (AH)
:   Example:

    ~~~~
      server AH accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * 51/any

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-AH]

    Notes

    > For more information see this
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#AH.ipsec)
    > and [RFC 2402](http://www.ietf.org/rfc/rfc2402.txt).
[WIKI-AH]: http://en.wikipedia.org/wiki/IPsec#Authentication_Header


## service: all

Match all traffic
:   Example:

    ~~~~
      server all accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * all

    Client Ports:

    * all

    Netfilter Modules

    * nf_conntrack_ftp [CONFIG_NF_CONNTRACK_FTP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_FTP.html)
    * nf_conntrack_irc [CONFIG_NF_CONNTRACK_IRC](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_IRC.html)
    * nf_conntrack_sip [CONFIG_NF_CONNTRACK_SIP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_SIP.html)
    * nf_conntrack_pptp [CONFIG_NF_CONNTRACK_PPTP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_PPTP.html)
    * nf_conntrack_proto_gre [CONFIG_NF_CT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_CT_PROTO_GRE.html)

    Netfilter NAT Modules

    * nf_nat_ftp [CONFIG_NF_NAT_FTP](http://cateee.net/lkddb/web-lkddb/NF_NAT_FTP.html)
    * nf_nat_irc [CONFIG_NF_NAT_IRC](http://cateee.net/lkddb/web-lkddb/NF_NAT_IRC.html)
    * nf_nat_sip [CONFIG_NF_NAT_SIP](http://cateee.net/lkddb/web-lkddb/NF_NAT_SIP.html)
    * nf_nat_pptp [CONFIG_NF_NAT_PPTP](http://cateee.net/lkddb/web-lkddb/NF_NAT_PPTP.html)
    * nf_nat_proto_gre [CONFIG_NF_NAT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_NAT_PROTO_GRE.html)

    Notes

    > Matches all traffic (all protocols, ports, etc.). Note that
    > to provide "connections in one direction with replies"
    > semantics, the kernel connection tracker is still used:
    > this will therefore still not match packets if they are
    > not understood as part of a connection (e.g. some
    > ICMPv6 packets, requests and replies taking different routes,
    > complex protocols with no helper loaded).
    > 
    > This service may indirectly setup a set of other services,
    > if they require kernel modules to be loaded.
    > The following complex services are activated:

## service: amanda

Advanced Maryland Automatic Network Disk Archiver
:   Service Type:

    * simple

    Server Ports:

    * udp/10080

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_amanda [CONFIG_NF_CONNTRACK_AMANDA](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_AMANDA.html)

    Netfilter NAT Modules

    * nf_nat_amanda [CONFIG_NF_NAT_AMANDA](http://cateee.net/lkddb/web-lkddb/NF_NAT_AMANDA.html)

    Links

    * [Homepage][HOME-amanda]
    * [Wikipedia][WIKI-amanda]

[HOME-amanda]: http://www.amanda.org/
[WIKI-amanda]: http://en.wikipedia.org/wiki/Advanced_Maryland_Automatic_Network_Disk_Archiver


## service: any

Match all traffic (without modules or indirect)
:   Example:

    ~~~~
      server any *myname* accept proto 47
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * all

    Client Ports:

    * all

    Netfilter Modules

    * 

    Netfilter NAT Modules

    * 

    Notes

    > Matches all traffic (all protocols, ports, etc), but does
    > not care about kernel modules and does not activate any
    > other service indirectly. In combination with the
    > [firehol-params(5)](#firehol-params5)
    > this service can match unusual traffic (e.g. GRE - protocol 47).
    > 
    > Note that you have to supply your own name in addition to
    > "any".

## service: anystateless

Match all traffic statelessly
:   Example:

    ~~~~
      server anystateless *myname* accept proto 47
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * all

    Client Ports:

    * all

    Notes

    > Matches all traffic (all protocols, ports, etc), but does
    > not care about kernel modules and does not activate any other
    > service indirectly. In combination with the
    > [firehol-params(5)](#firehol-params5)
    > this service can match unusual traffic (e.g. GRE - protocol 47).
    > 
    > This service is identical to "any" but does not care about
    > the state of traffic.
    > 
    > Note that you have to supply your own name in addition to
    > "anystateless".

## service: apcupsd

APC UPS Daemon
:   Example:

    ~~~~
      server apcupsd accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/6544

    Client Ports:

    * default

    Links

    * [Homepage][HOME-apcupsd]
    * [Wikipedia][WIKI-apcupsd]

    Notes

    > This service must be defined as "server apcupsd accept" on
    > all machines not directly connected to the UPS (i.e. slaves).
    > 
    > Note that the port defined here is not the default port (6666)
    > used if you download and compile APCUPSD, since the default
    > conflicts with IRC and many distributions (like Debian) have
    > changed this to 6544.
    > 
    > You can define port 6544 in APCUPSD, by changing the value
    > of NETPORT in its configuration file, or overwrite this
    > FireHOL service definition using the procedures described
    > in [Adding Services](#adding-services)
    > in [firehol.conf(5)](#firehol.conf5).
[HOME-apcupsd]: http://www.apcupsd.com
[WIKI-apcupsd]: http://en.wikipedia.org/wiki/Apcupsd


## service: apcupsdnis

APC UPS Daemon Network Information Server
:   Example:

    ~~~~
      server apcupsdnis accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3551

    Client Ports:

    * default

    Links

    * [Homepage][HOME-apcupsdnis]
    * [Wikipedia][WIKI-apcupsdnis]

    Notes

    > This service allows the remote WEB interfaces of
    > [APCUPSD](http://www.apcupsd.com/), to connect
    > and get information from the server directly connected to
    > the UPS device.
[HOME-apcupsdnis]: http://www.apcupsd.com
[WIKI-apcupsdnis]: http://en.wikipedia.org/wiki/Apcupsd


## service: aptproxy

Advanced Packaging Tool Proxy
:   Example:

    ~~~~
      server aptproxy accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/9999

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-aptproxy]

[WIKI-aptproxy]: http://en.wikipedia.org/wiki/Apt-proxy


## service: asterisk

Asterisk PABX
:   Example:

    ~~~~
      server asterisk accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5038

    Client Ports:

    * default

    Links

    * [Homepage][HOME-asterisk]
    * [Wikipedia][WIKI-asterisk]

    Notes

    > This service refers only to the manager interface of asterisk.
    > You should normally enable
    > [sip][keyword-service-sip],
    > [h323][keyword-service-h323],
    > [rtp][keyword-service-rtp], etc. at the
    > firewall level, if you enable the relative channel drivers
    > of asterisk.
[HOME-asterisk]: http://www.asterisk.org
[WIKI-asterisk]: http://en.wikipedia.org/wiki/Asterisk_PBX


## service: cups

Common UNIX Printing System
:   Example:

    ~~~~
      server cups accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/631 udp/631

    Client Ports:

    * any

    Links

    * [Homepage][HOME-cups]
    * [Wikipedia][WIKI-cups]

[HOME-cups]: http://www.cups.org
[WIKI-cups]: http://en.wikipedia.org/wiki/Common_Unix_Printing_System


## service: custom

Custom definitions
:   Example:

    ~~~~
      server custom myimap tcp/143 default accept
    ~~~~

    Service Type:

    * custom

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Notes

    > The full syntax is:
    > 
    > *subcommand* `custom` *name* *svr-proto/ports* *cli-ports* *action* *params*
    > 
    > This service is used by FireHOL to allow you create rules
    > for services which do not have a definition.
    > 
    > `subcommand`, *action* and *params* have their usual meanings.
    > 
    > A *name* must be supplied along with server
    > ports in the form *proto/range*
    > and client ports which takes only a
    > *range*.
    > 
    > To define services with the built-in extension mechanism
    > to avoid the need for `custom` services,
    > see [Adding Services](#adding-services)
    > in [firehol.conf(5)](#firehol.conf5).

## service: cvspserver

Concurrent Versions System
:   Example:

    ~~~~
      server cvspserver accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/2401

    Client Ports:

    * default

    Links

    * [Homepage][HOME-cvspserver]
    * [Wikipedia][WIKI-cvspserver]

[HOME-cvspserver]: http://www.nongnu.org/cvs/
[WIKI-cvspserver]: http://en.wikipedia.org/wiki/Concurrent_Versions_System


## service: darkstat

Darkstat network traffic analyser
:   Example:

    ~~~~
      server darkstat accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/666

    Client Ports:

    * default

    Links

    * [Homepage][HOME-darkstat]

[HOME-darkstat]: https://unix4lyfe.org/darkstat/


## service: daytime

Daytime Protocol
:   Example:

    ~~~~
      server daytime accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/13

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-daytime]

[WIKI-daytime]: http://en.wikipedia.org/wiki/Daytime_Protocol


## service: dcc

Distributed Checksum Clearinghouse
:   Example:

    ~~~~
      server dcc accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/6277

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-dcc]

    Notes

    > See also this
    > [DCC FAQ](http://www.rhyolite.com/dcc/FAQ.html#firewall-ports).
[WIKI-dcc]: http://en.wikipedia.org/wiki/Distributed_Checksum_Clearinghouse


## service: dcpp

Direct Connect++ P2P
:   Example:

    ~~~~
      server dcpp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1412 udp/1412

    Client Ports:

    * default

    Links

    * [Homepage][HOME-dcpp]

[HOME-dcpp]: http://dcplusplus.sourceforge.net


## service: dhcp

Dynamic Host Configuration Protocol
:   Example:

    ~~~~
      server dhcp accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * udp/67

    Client Ports:

    * 68

    Links

    * [Wikipedia][WIKI-dhcp]

    Notes

    > The dhcp service is implemented as stateless rules.
    > 
    > DHCP clients broadcast to the network (src 0.0.0.0
    > dst 255.255.255.255) to find a DHCP server. If the DHCP
    > service was stateful the iptables connection tracker would
    > not match the packets and deny to send the reply.
    > 
    > Note that this change does not affect the security of either
    > DHCP servers or clients, since only the specific ports are
    > allowed (there is no random port at either the server or the
    > client side).
    > 
    > Note also that the "server dhcp accept" or "client dhcp accept"
    > commands should placed within interfaces that do not
    > have src and / or dst defined (because of the initial
    > broadcast).
    > 
    > You can overcome this problem by placing the DHCP service on
    > a separate interface, without a src or dst but with a policy
    > return. Place this interface before the one that defines the
    > rest of the services.
    > 
    > For example:
    > 
    > `interface eth0 dhcp`
    > 
    > `    policy return`
    > 
    > `    server dhcp accept`
    > 
    > 
    > `interface eth0 lan src "$mylan" dst "$myip"`
    > 
    > `    client all accept`
    > 
    > For example:
    > interface eth0 dhcp
    > policy return
    > server dhcp accept
    > interface eth0 lan src "$mylan" dst "$myip"
    > client all accept
    > 
    > This service implicitly sets its client or server to ipv4 mode.
[WIKI-dhcp]: http://en.wikipedia.org/wiki/Dhcp


## service: dhcprelay

DHCP Relay
:   Example:

    ~~~~
      server dhcprelay accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/67

    Client Ports:

    * 67

    Links

    * [Wikipedia][WIKI-dhcprelay]

    Notes

    > From RFC 1812 section 9.1.2:
    > 
    > In many cases, BOOTP clients and their associated BOOTP
    > server(s) do not reside on the same IP (sub)network. In
    > such cases, a third-party agent is required to transfer
    > BOOTP messages between clients and servers. Such an agent
    > was originally referred to as a BOOTP forwarding agent.
    > However, to avoid confusion with the IP forwarding function
    > of a router, the name BOOTP relay agent has been adopted
    > instead.
    > 
    > For more information about DHCP Relay see section 9.1.2 of
    > [RFC 1812](http://www.ietf.org/rfc/rfc1812.txt)
    > and section 4 of
    > [RFC 1542](http://www.ietf.org/rfc/rfc1542.txt)
[WIKI-dhcprelay]: http://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol#DHCP_relaying


## service: dhcpv6

Dynamic Host Configuration Protocol for IPv6
:   Example:

    ~~~~
      server dhcp accept
      client dhcp accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * udp/547

    Client Ports:

    * udp/546

    Links

    * [Wikipedia][WIKI-dhcpv6]

    Notes

    > The dhcp service is implemented as stateless rules.
    > It cannot be stateful as the connection tracker will not
    > match a unicast reply to a broadcast request. Further,
    > if you wish to add src/dst rule parameters, you must
    > account for both the broadcast and link-local network prefixes.
    > 
    > Clients broadcast from a link-local address to the
    > multicast address ff02::1:2 on UDP port 547 to find a
    > server. The server sends a unicast reply back to the
    > client which listens on UDP port 546.
    > 
    > For a FireHOL interface, creating a client will allow
    > sending to port 547 and receiving on port 546. Creating
    > a server allows sending to port 546 and receiving on port 547.
    > 
    > Unlike DHCP for IPv4, the source ports to be used are not
    > defined in DHCPv6 - see section 5.2 of
    > [RFC3315](http://www.ietf.org/rfc/rfc3315.txt).
    > Some servers are known to make use of this to send from
    > arbitrary ports, so FireHOL does not assume a source port.
    > 
    > This service implicitly sets its client or server to ipv6 mode.
[WIKI-dhcpv6]: https://en.wikipedia.org/wiki/DHCPv6


## service: dict

Dictionary Server Protocol
:   Example:

    ~~~~
      server dict accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/2628

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-dict]

    Notes

    > See
    > [RFC2229](http://www.ietf.org/rfc/rfc2229.txt).
[WIKI-dict]: http://en.wikipedia.org/wiki/DICT


## service: distcc

Distributed CC
:   Example:

    ~~~~
      server distcc accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3632

    Client Ports:

    * default

    Links

    * [Homepage][HOME-distcc]
    * [Wikipedia][WIKI-distcc]

    Notes

    > For distcc security, please check the
    > [distcc security design](http://distcc.googlecode.com/svn/trunk/doc/web/security.html).
[HOME-distcc]: https://code.google.com/p/distcc/
[WIKI-distcc]: http://en.wikipedia.org/wiki/Distcc


## service: dns

Domain Name System
:   Example:

    ~~~~
      server dns accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/53 tcp/53

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-dns]

    Notes

    > On very busy DNS servers you may see a few dropped DNS
    > packets in your logs. This is normal. The iptables
    > connection tracker will timeout the session and lose
    > unmatched DNS packets that arrive too late to be useful.
[WIKI-dns]: http://en.wikipedia.org/wiki/Domain_Name_System


## service: echo

Echo Protocol
:   Example:

    ~~~~
      server echo accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/7

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-echo]

[WIKI-echo]: http://en.wikipedia.org/wiki/Echo_Protocol


## service: emule

eMule (Donkey network client)
:   Example:

    ~~~~
      client emule accept src 192.0.2.1
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * many

    Links

    * [Homepage][HOME-emule]

    Notes

    > According to
    > [eMule Port Definitions](http://www.emule-project.net/home/perl/help.cgi?l=1&rm=show_topic&topic_id=122),
    > FireHOL defines:
    > 
    > * Accept from any client port to the server at tcp/4661
    > * Accept from any client port to the server at tcp/4662
    > * Accept from any client port to the server at udp/4665
    > * Accept from any client port to the server at udp/4672
    > * Accept from any server port to the client at tcp/4662
    > * Accept from any server port to the client at udp/4672
    > 
    > Use the FireHOL [firehol-client(5)][keyword-firehol-client]
    > command to match the eMule client.
    > 
    > Please note that the eMule client is an HTTP client also.
[HOME-emule]: http://www.emule-project.com


## service: eserver

eDonkey network server
:   Example:

    ~~~~
      server eserver accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/4661 udp/4661 udp/4665

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-eserver]

[WIKI-eserver]: http://en.wikipedia.org/wiki/Eserver


## service: ESP

IPSec Encapsulated Security Payload (ESP)
:   Example:

    ~~~~
      server ESP accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * 50/any

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ESP]

    Notes

    > For more information see this
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#ESP.ipsec)
    > [RFC 2406](http://www.ietf.org/rfc/rfc2406.txt).
[WIKI-ESP]: http://en.wikipedia.org/wiki/IPsec#Encapsulating_Security_Payload


## service: finger

Finger Protocol
:   Example:

    ~~~~
      server finger accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/79

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-finger]

[WIKI-finger]: http://en.wikipedia.org/wiki/Finger_protocol


## service: ftp

File Transfer Protocol
:   Example:

    ~~~~
      server ftp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/21

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_ftp [CONFIG_NF_CONNTRACK_FTP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_FTP.html)

    Netfilter NAT Modules

    * nf_nat_ftp [CONFIG_NF_NAT_FTP](http://cateee.net/lkddb/web-lkddb/NF_NAT_FTP.html)

    Links

    * [Wikipedia][WIKI-ftp]

    Notes

    > The FTP service matches both active and passive FTP
    > connections.
[WIKI-ftp]: http://en.wikipedia.org/wiki/Ftp


## service: gift

giFT Internet File Transfer
:   Example:

    ~~~~
      server gift accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/4302 tcp/1214 tcp/2182 tcp/2472

    Client Ports:

    * any

    Links

    * [Homepage][HOME-gift]
    * [Wikipedia][WIKI-gift]

    Notes

    > The gift FireHOL service supports:
    > 
    > * Gnutella listening at tcp/4302
    > * FastTrack listening at tcp/1214
    > * OpenFT listening at tcp/2182 and tcp/2472
    > 
    > The above ports are the defaults given for the corresponding
    > giFT modules.
    > 
    > To allow access to the user interface ports of giFT, use
    > the [giftui][keyword-service-giftui].
[HOME-gift]: http://gift.sourceforge.net
[WIKI-gift]: http://en.wikipedia.org/wiki/GiFT


## service: giftui

giFT Internet File Transfer User Interface
:   Example:

    ~~~~
      server giftui accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1213

    Client Ports:

    * default

    Links

    * [Homepage][HOME-giftui]
    * [Wikipedia][WIKI-giftui]

    Notes

    > This service refers only to the user interface ports offered
    > by giFT. To allow gift accept P2P requests, use the
    > [gift][keyword-service-gift].
[HOME-giftui]: http://gift.sourceforge.net
[WIKI-giftui]: http://en.wikipedia.org/wiki/GiFT


## service: gkrellmd

GKrellM Daemon
:   Example:

    ~~~~
      server gkrellmd accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/19150

    Client Ports:

    * default

    Links

    * [Homepage][HOME-gkrellmd]
    * [Wikipedia][WIKI-gkrellmd]

[HOME-gkrellmd]: http://gkrellm.net/
[WIKI-gkrellmd]: http://en.wikipedia.org/wiki/Gkrellm


## service: GRE

Generic Routing Encapsulation
:   Example:

    ~~~~
      server GRE accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * 47/any

    Client Ports:

    * any

    Netfilter Modules

    * nf_conntrack_proto_gre [CONFIG_NF_CT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_CT_PROTO_GRE.html)

    Netfilter NAT Modules

    * nf_nat_proto_gre [CONFIG_NF_NAT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_NAT_PROTO_GRE.html)

    Links

    * [Wikipedia][WIKI-GRE]

    Notes

    > Protocol No 47.
    > 
    > For more information see RFC [RFC 2784](http://www.ietf.org/rfc/rfc2784.txt).
[WIKI-GRE]: http://en.wikipedia.org/wiki/Generic_Routing_Encapsulation


## service: h323

H.323 VoIP
:   Example:

    ~~~~
      server h323 accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1720 tcp/1720

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_h323 [CONFIG_NF_CONNTRACK_H323](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_H323.html)

    Netfilter NAT Modules

    * nf_nat_h323 [CONFIG_NF_NAT_H323](http://cateee.net/lkddb/web-lkddb/NF_NAT_H323.html)

    Links

    * [Wikipedia][WIKI-h323]

[WIKI-h323]: http://en.wikipedia.org/wiki/H323


## service: heartbeat

HeartBeat
:   Example:

    ~~~~
      server heartbeat accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/690:699

    Client Ports:

    * default

    Links

    * [Homepage][HOME-heartbeat]

    Notes

    > This FireHOL service has been designed such a way that it
    > will allow multiple heartbeat clusters on the same LAN.
[HOME-heartbeat]: http://www.linux-ha.org/


## service: http

Hypertext Transfer Protocol
:   Example:

    ~~~~
      server http accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/80

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-http]

[WIKI-http]: http://en.wikipedia.org/wiki/Http


## service: httpalt

HTTP alternate port
:   Example:

    ~~~~
      server httpalt accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/8080

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-httpalt]

    Notes

    > This port is commonly used by web servers, web proxies
    > and caches where the standard [http][keyword-service-http]
    > port is not available or can or should not be used.
[WIKI-httpalt]: http://en.wikipedia.org/wiki/Http


## service: https

Secure Hypertext Transfer Protocol
:   Example:

    ~~~~
      server https accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/443

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-https]

[WIKI-https]: http://en.wikipedia.org/wiki/Https


## service: hylafax

HylaFAX
:   Example:

    ~~~~
      server hylafax accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * many

    Links

    * [Homepage][HOME-hylafax]
    * [Wikipedia][WIKI-hylafax]

    Notes

    > This service allows incoming requests to server port
    > tcp/4559 and outgoing from server port tcp/4558.
    > 
    > The correct operation of this service has not been verified.
    > 
    > USE THIS WITH CARE. A HYLAFAX CLIENT MAY OPEN ALL TCP
    > UNPRIVILEGED PORTS TO ANYONE (from port tcp/4558).
[HOME-hylafax]: http://www.hylafax.org/
[WIKI-hylafax]: http://en.wikipedia.org/wiki/Hylafax


## service: iax

Inter-Asterisk eXchange
:   Example:

    ~~~~
      server iax accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/5036

    Client Ports:

    * default

    Links

    * [Homepage][HOME-iax]
    * [Wikipedia][WIKI-iax]

    Notes

    > This service refers to IAX version 1.
    > There is also [iax2][keyword-service-iax2].
[HOME-iax]: http://www.asterisk.org
[WIKI-iax]: http://en.wikipedia.org/wiki/Iax


## service: iax2

Inter-Asterisk eXchange v2
:   Example:

    ~~~~
      server iax2 accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/5469 udp/4569

    Client Ports:

    * default

    Links

    * [Homepage][HOME-iax2]
    * [Wikipedia][WIKI-iax2]

    Notes

    > This service refers to IAX version 2.
    > There is also [iax][keyword-service-iax].
[HOME-iax2]: http://www.asterisk.org
[WIKI-iax2]: http://en.wikipedia.org/wiki/Iax


## service: ICMP

Internet Control Message Protocol
:   Example:

    ~~~~
      server ICMP accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * icmp/any

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ICMP]

[WIKI-ICMP]: http://en.wikipedia.org/wiki/Internet_Control_Message_Protocol


## service: icmp

Internet Control Message Protocol
:   Alias for [ICMP][keyword-service-ICMP]


## service: ICMPV6

Internet Control Message Protocol v6
:   Example:

    ~~~~
      server ICMPV6 accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * icmpv6/any

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ICMPV6]

[WIKI-ICMPV6]: http://en.wikipedia.org/wiki/ICMPv6


## service: icmpv6

Internet Control Message Protocol v6
:   Alias for [ICMPV6][keyword-service-ICMPV6]


## service: icp

Internet Cache Protocol
:   Example:

    ~~~~
      server icp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/3130

    Client Ports:

    * 3130

    Links

    * [Wikipedia][WIKI-icp]

[WIKI-icp]: http://en.wikipedia.org/wiki/Internet_Cache_Protocol


## service: ident

Identification Protocol
:   Example:

    ~~~~
      server ident reject with tcp-reset
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/113

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-ident]

[WIKI-ident]: http://en.wikipedia.org/wiki/Ident_protocol


## service: imap

Internet Message Access Protocol
:   Example:

    ~~~~
      server imap accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/143

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-imap]

[WIKI-imap]: http://en.wikipedia.org/wiki/Imap


## service: imaps

Secure Internet Message Access Protocol
:   Example:

    ~~~~
      server imaps accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/993

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-imaps]

[WIKI-imaps]: http://en.wikipedia.org/wiki/Imap


## service: ipsecnatt

NAT traversal and IPsec
:   Service Type:

    * simple

    Server Ports:

    * udp/4500

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ipsecnatt]

[WIKI-ipsecnatt]: http://en.wikipedia.org/wiki/NAT_traversal#IPsec_traversal_across_NAT


## service: ipv6error

ICMPv6 Error Handling
:   Example:

    ~~~~
      server ipv6error accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Notes

    > Not all icmpv6 error types should be treated equally
    > inbound and outbound.
    > 
    > The ipv6error rule wraps all of them in the following way:
    > * allow incoming messages only for existing sessions
    > * allow outgoing messages always
    > 
    > The following ICMPv6 messages are handled:
    > 
    > * destination-unreachable
    > * packet-too-big
    > * ttl-zero-during-transit
    > * ttl-zero-during-reassembly
    > * unknown-header-type
    > * unknown-option
    > 
    > Interfaces should always have this set:
    > 
    > `server ipv6error accept`
    > 
    > In a router with inface being internal and outface being
    > external the following will meet the recommendations of
    > [RFC 4890](http://tools.ietf.org/html/rfc4890):
    > 
    > `server ipv6error accept`
    > 
    > Do not use:
    > `client ipv6error accept`
    > unless you are controlling traffic on a router interface
    > where outface is the internal destination.
    > 
    > This service implicitly sets its client or server to ipv6 mode.

## service: ipv6mld

IPv6 Multicast Listener Discovery for IPv6
:   Example:

    ~~~~
      client ipv6mld accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-ipv6mld]

    Notes

    > IPv6 uses Multicast Listener Discovery to discover multicast
    > listeners and what they are listening for.
    > 
    > In practice all IPv6 nodes are multicast listeners since
    > multicast is used in the neighbour discovery protocol which
    > replaces ARP in IPv4.
    > 
    > These rules are stateless since reports can happen
    > automatically as well as on query.
    > 
    > Unless muticast snooping is disabled across the network, MLD
    > should be enabled for any clients:
    > 
    > `client ipv6mld accept`
    > 
    > MLD should also be enabled as a server on any hosts acting as a
    > router:
    > 
    > `server ipv6mld accept`
    > 
    > The rules should generally not be used to pass packets across a
    > firewall (e.g. in a router definition) unless the firewall
    > is for a bridge.
    > 
    > This service implicitly sets its client or server to ipv6 mode.
[WIKI-ipv6mld]: https://en.wikipedia.org/wiki/Multicast_Listener_Discovery


## service: ipv6neigh

IPv6 Neighbour discovery
:   Example:

    ~~~~
      client ipv6neigh accept
      server ipv6neigh accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-ipv6neigh]

    Notes

    > IPv6 uses the Neighbour Discovery Protocol to do automatic
    > configuration of routes and to replace ARP. To allow this
    > functionality the network neighbour and router
    > solicitation/advertisement messages should be enabled on
    > each interface.
    > 
    > These rules are stateless since advertisement can happen
    > automatically as well as on solicitation.
    > 
    > Neighbour discovery (incoming) should always be enabled:
    > 
    > `server ipv6neigh accept`
    > 
    > Neighbour advertisement (outgoing) should always be enabled:
    > 
    > `client ipv6neigh accept`
    > 
    > The rules should not be used to pass packets across a
    > firewall (e.g. in a router definition) unless the firewall
    > is for a bridge.
    > 
    > This service implicitly sets its client or server to ipv6 mode.
[WIKI-ipv6neigh]: https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol


## service: ipv6router

IPv6 Router discovery
:   Example:

    ~~~~
      client ipv6router accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-ipv6router]

    Notes

    > IPv6 uses the Neighbour Discovery Protocol to do automatic
    > configuration of routes and to replace ARP. To allow this
    > functionality the network neighbour and router
    > solicitation/advertisement messages should be enabled on
    > each interface.
    > 
    > These rules are stateless since advertisement can happen
    > automatically as well as on solicitation.
    > 
    > Router discovery (incoming) should always be enabled:
    > 
    > `client ipv6router accept`
    > 
    > Router advertisement (outgoing) should be enabled on
    > a host that routes:
    > 
    > `server ipv6router accept`
    > 
    > The rules should not be used to pass packets across a
    > firewall (e.g. in a router definition) unless the firewall
    > is for a bridge.
    > 
    > This service implicitly sets its client or server to ipv6 mode.
[WIKI-ipv6router]: https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol


## service: irc

Internet Relay Chat
:   Example:

    ~~~~
      server irc accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/6667

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_irc [CONFIG_NF_CONNTRACK_IRC](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_IRC.html)

    Netfilter NAT Modules

    * nf_nat_irc [CONFIG_NF_NAT_IRC](http://cateee.net/lkddb/web-lkddb/NF_NAT_IRC.html)

    Links

    * [Wikipedia][WIKI-irc]

[WIKI-irc]: http://en.wikipedia.org/wiki/Internet_Relay_Chat


## service: isakmp

Internet Security Association and Key Management Protocol (IKE)
:   Example:

    ~~~~
      server isakmp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/500

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-isakmp]

    Notes

    > For more information see the
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#IKE.ipsec)
[WIKI-isakmp]: http://en.wikipedia.org/wiki/ISAKMP


## service: jabber

Extensible Messaging and Presence Protocol
:   Example:

    ~~~~
      server jabber accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5222 tcp/5223

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-jabber]

    Notes

    > Allows clear and SSL client-to-server connections.
[WIKI-jabber]: http://en.wikipedia.org/wiki/Jabber


## service: jabberd

Extensible Messaging and Presence Protocol (Server)
:   Example:

    ~~~~
      server jabberd accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5222 tcp/5223 tcp/5269

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-jabberd]

    Notes

    > Allows clear and SSL client-to-server and server-to-server
    > connections.
    > 
    > Use this service for a jabberd server. In all other cases,
    > use the [jabber][keyword-service-jabber].
[WIKI-jabberd]: http://en.wikipedia.org/wiki/Jabber


## service: l2tp

Layer 2 Tunneling Protocol
:   Service Type:

    * simple

    Server Ports:

    * udp/1701

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-l2tp]

[WIKI-l2tp]: http://en.wikipedia.org/wiki/L2tp


## service: ldap

Lightweight Directory Access Protocol
:   Example:

    ~~~~
      server ldap accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/389

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-ldap]

[WIKI-ldap]: http://en.wikipedia.org/wiki/Ldap


## service: ldaps

Secure Lightweight Directory Access Protocol
:   Example:

    ~~~~
      server ldaps accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/636

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-ldaps]

[WIKI-ldaps]: http://en.wikipedia.org/wiki/Ldap


## service: lpd

Line Printer Daemon Protocol
:   Example:

    ~~~~
      server lpd accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/515

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-lpd]

    Notes

    > LPD is documented in
    > [RFC 1179](http://www.ietf.org/rfc/rfc1179.txt).
    > 
    > Since many operating systems incorrectly use the non-default
    > client ports for LPD access, this definition allows any
    > client port to access the service (in addition to
    > the RFC defined 721 to 731 inclusive).
[WIKI-lpd]: http://en.wikipedia.org/wiki/Line_Printer_Daemon_protocol


## service: microsoft_ds

Direct Hosted (NETBIOS-less) SMB
:   Example:

    ~~~~
      server microsoft_ds accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/445

    Client Ports:

    * default

    Notes

    > Direct Hosted (i.e. NETBIOS-less SMB)
    > 
    > This is another NETBIOS Session Service with minor
    > differences with [netbios_ssn][keyword-service-netbios_ssn].
    > It is supported only by Windows 2000 and Windows XP and
    > it offers the advantage of being independent of WINS for
    > name resolution.
    > 
    > It seems that samba supports transparently this protocol
    > on the [netbios_ssn][keyword-service-netbios_ssn] ports, so
    > that either direct hosted or traditional SMB can be served
    > simultaneously.
    > 
    > Please refer to the [netbios_ssn][keyword-service-netbios_ssn]
    > for more information.

## service: mms

Microsoft Media Server
:   Example:

    ~~~~
      server mms accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1755 udp/1755

    Client Ports:

    * default

    Netfilter Modules

    * See [here](http://www.netfilter.org/documentation/HOWTO/netfilter-extensions-HOWTO-5.html#ss5.5).

    Netfilter NAT Modules

    * See [here](http://www.netfilter.org/documentation/HOWTO/netfilter-extensions-HOWTO-5.html#ss5.5).

    Links

    * [Wikipedia][WIKI-mms]

    Notes

    > Microsoft's proprietary network streaming protocol used
    > to transfer unicast data in Windows Media Services
    > (previously called NetShow Services).
[WIKI-mms]: http://en.wikipedia.org/wiki/Microsoft_Media_Server


## service: msn

Microsoft MSN Messenger Service
:   Example:

    ~~~~
      server msn accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1863 udp/1863

    Client Ports:

    * default


## service: msnp

msnp
:   Example:

    ~~~~
      server msnp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/6891

    Client Ports:

    * default


## service: ms_ds

Direct Hosted (NETBIOS-less) SMB
:   Alias for [microsoft_ds][keyword-service-microsoft_ds]


## service: multicast

Multicast
:   Example:

    ~~~~
      server multicast reject with proto-unreach
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-multicast]

    Notes

    > The multicast service matches all packets sent to
    > the $MULTICAST_IPS addresses using IGMP or UDP.
    > For IPv4 that means 224.0.0.0/4 and for IPv6 FF00::/16.
[WIKI-multicast]: http://en.wikipedia.org/wiki/Multicast


## service: mysql

MySQL
:   Example:

    ~~~~
      server mysql accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3306

    Client Ports:

    * default

    Links

    * [Homepage][HOME-mysql]
    * [Wikipedia][WIKI-mysql]

[HOME-mysql]: http://www.mysql.com/
[WIKI-mysql]: http://en.wikipedia.org/wiki/Mysql


## service: netbackup

Veritas NetBackup service
:   Example:

    ~~~~
      server netbackup accept
      client netbackup accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/13701 tcp/13711 tcp/13720 tcp/13721 tcp/13724 tcp/13782 tcp/13783

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-netbackup]

    Notes

    > To use this service you must define it as both client and
    > server in NetBackup clients and NetBackup servers.
[WIKI-netbackup]: http://en.wikipedia.org/wiki/Netbackup


## service: netbios_dgm

NETBIOS Datagram Distribution Service
:   Example:

    ~~~~
      server netbios_dgm accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/138

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-netbios_dgm]

    Notes

    > See also the [samba][keyword-service-samba].
    > 
    > Keep in mind that this service broadcasts (to the broadcast
    > address of your LAN) UDP packets. If you place this service
    > within an interface that has a dst parameter, remember to
    > include (in the dst parameter) the broadcast address of your
    > LAN too.
[WIKI-netbios_dgm]: http://en.wikipedia.org/wiki/Netbios#Datagram_distribution_service


## service: netbios_ns

NETBIOS Name Service
:   Example:

    ~~~~
      server netbios_ns accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/137

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-netbios_ns]

    Notes

    > See also the [samba][keyword-service-samba].
[WIKI-netbios_ns]: http://en.wikipedia.org/wiki/Netbios#Name_service


## service: netbios_ssn

NETBIOS Session Service
:   Example:

    ~~~~
      server netbios_ssn accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/139

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-netbios_ssn]

    Notes

    > See also the [samba][keyword-service-samba].
    > 
    > Please keep in mind that newer NETBIOS clients prefer to use
    > port 445 ([microsoft_ds][keyword-service-microsoft_ds]) for the
    > NETBIOS session service, and when this is not available they
    > fall back to port 139 (netbios_ssn). Versions of samba above
    > 3.x bind automatically to ports 139 and 445.
    > 
    > If you have an older samba version and your policy on an
    > interface or router is DROP, clients trying to access port
    > 445 will have to timeout before falling back to port 139.
    > This timeout can be up to several minutes.
    > 
    > To overcome this problem you can explicitly REJECT the
    > [microsoft_ds][keyword-service-microsoft_ds] with a
    > tcp-reset message:
    > 
    > server microsoft_ds reject with tcp-reset
[WIKI-netbios_ssn]: http://en.wikipedia.org/wiki/Netbios#Session_service


## service: nfs

Network File System
:   Example:

    ~~~~
      client nfs accept dst 192.0.2.1
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-nfs]

    Notes

    > The NFS service queries the RPC service on the NFS server
    > host to find out the ports nfsd, mountd, lockd and rquotad
    > are listening. Then, according to these ports it sets up
    > rules on all the supported protocols (as reported by RPC)
    > in order the clients to be able to reach the server.
    > 
    > For this reason, the NFS service requires that:
    > 
    > * the firewall is restarted if the NFS server is restarted
    > * the NFS server must be specified on all nfs statements (only if it is not the localhost)
    > 
    > Since NFS queries the remote RPC server, it is required to
    > also be allowed to do so, by allowing the
    > [portmap][keyword-service-portmap] too. Take care that
    > this is allowed by the running firewall when FireHOL tries
    > to query the RPC server. So you might have to setup NFS in
    > two steps: First add the portmap service and activate the
    > firewall, then add the NFS service and restart the firewall.
    > 
    > To avoid this you can setup your NFS server to listen on
    > pre-defined ports, as documented in
    > [NFS Howto][NFS Howto].
    > If you do this then you will have to define the the ports
    > using the procedure described
    > in [Adding Services](#adding-services)
    > in [firehol.conf(5)](#firehol.conf5).
    > 
    > [NFS Howto]: http://nfs.sourceforge.net/nfs-howto/ar01s06.html#nfs_firewalls
[WIKI-nfs]: http://en.wikipedia.org/wiki/Network_File_System_%28protocol%29


## service: nis

Network Information Service
:   Example:

    ~~~~
      client nis accept dst 192.0.2.1
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-nis]

    Notes

    > The nis service queries the RPC service on the nis server
    > host to find out the ports ypserv and yppasswdd are listening.
    > Then, according to these ports it sets up rules on all the
    > supported protocols (as reported by RPC) in order the clients
    > to be able to reach the server.
    > 
    > For this reason, the nis service requires that:
    > 
    > * the firewall is restarted if the nis server is restarted
    > * the nis server must be specified on all nis statements (only if it is not the localhost)
    > 
    > Since nis queries the remote RPC server, it is required to
    > also be allowed to do so, by allowing the
    > [portmap][keyword-service-portmap] too. Take care that
    > this is allowed by the running firewall when FireHOL tries
    > to query the RPC server. So you might have to setup nis in
    > two steps: First add the portmap service and activate the
    > firewall, then add the nis service and restart the firewall.
    > 
    > This service was added to FireHOL by
    > [Carlos Rodrigues](http://sourceforge.net/p/firehol/feature-requests/20/).
    > His comments regarding this implementation, are:
    > 
    > These rules work for client access only!
    > 
    > Pushing changes to slave servers won't work if these rules
    > are active somewhere between the master and its slaves,
    > because it is impossible to predict the ports where yppush
    > will be listening on each push.
    > 
    > Pulling changes directly on the slaves will work, and could
    > be improved performance-wise if these rules are modified to
    > open fypxfrd. This wasn't done because it doesn't make that
    > much sense since pushing changes on the master server is
    > the most common, and recommended, way to replicate maps.
[WIKI-nis]: http://en.wikipedia.org/wiki/Network_Information_Service


## service: nntp

Network News Transfer Protocol
:   Example:

    ~~~~
      server nntp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/119

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-nntp]

[WIKI-nntp]: http://en.wikipedia.org/wiki/Nntp


## service: nntps

Secure Network News Transfer Protocol
:   Example:

    ~~~~
      server nntps accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/563

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-nntps]

[WIKI-nntps]: http://en.wikipedia.org/wiki/Nntp


## service: nrpe

Nagios NRPE
:   Service Type:

    * simple

    Server Ports:

    * tcp/5666

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-nrpe]

[WIKI-nrpe]: http://en.wikipedia.org/wiki/Nagios#NRPE


## service: ntp

Network Time Protocol
:   Example:

    ~~~~
      server ntp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/123 tcp/123

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ntp]

[WIKI-ntp]: http://en.wikipedia.org/wiki/Network_Time_Protocol


## service: nut

Network UPS Tools
:   Example:

    ~~~~
      server nut accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3493 udp/3493

    Client Ports:

    * default

    Links

    * [Homepage][HOME-nut]

[HOME-nut]: http://www.networkupstools.org/


## service: nxserver

NoMachine NX Server
:   Example:

    ~~~~
      server nxserver accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5000:5200

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-nxserver]

    Notes

    > Default ports used by NX server for connections without
    > encryption.
    > 
    > Note that nxserver also needs the [ssh][keyword-service-ssh]
    > to be enabled.
    > 
    > This information has been extracted from this
    > The TCP ports used by nxserver are
    > 4000 + DISPLAY_BASE to 4000 + DISPLAY_BASE + DISPLAY_LIMIT.
    > DISPLAY_BASE and DISPLAY_LIMIT are set in /usr/NX/etc/node.conf
    > and the defaults are DISPLAY_BASE=1000 and DISPLAY_LIMIT=200.
    > 
    > For encrypted nxserver sessions, only
    > [ssh][keyword-service-ssh] is needed.
[WIKI-nxserver]: http://en.wikipedia.org/wiki/NX_Server


## service: openvpn

OpenVPN
:   Service Type:

    * simple

    Server Ports:

    * tcp/1194 udp/1194

    Client Ports:

    * default

    Links

    * [Homepage][HOME-openvpn]
    * [Wikipedia][WIKI-openvpn]

[HOME-openvpn]: http://openvpn.net/
[WIKI-openvpn]: http://en.wikipedia.org/wiki/OpenVPN


## service: oracle

Oracle Database
:   Example:

    ~~~~
      server oracle accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1521

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-oracle]

[WIKI-oracle]: http://en.wikipedia.org/wiki/Oracle_db


## service: OSPF

Open Shortest Path First
:   Example:

    ~~~~
      server OSPF accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * 89/any

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-OSPF]

[WIKI-OSPF]: http://en.wikipedia.org/wiki/Ospf


## service: ping

Ping (ICMP echo)
:   Example:

    ~~~~
      server ping accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-ping]

    Notes

    > This services matches requests of protocol ICMP and type
    > echo-request (TYPE=8) and their replies of type echo-reply
    > (TYPE=0).
    > 
    > The ping service is stateful.
[WIKI-ping]: http://en.wikipedia.org/wiki/Ping


## service: pop3

Post Office Protocol
:   Example:

    ~~~~
      server pop3 accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/110

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-pop3]

[WIKI-pop3]: http://en.wikipedia.org/wiki/Pop3


## service: pop3s

Secure Post Office Protocol
:   Example:

    ~~~~
      server pop3s accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/995

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-pop3s]

[WIKI-pop3s]: http://en.wikipedia.org/wiki/Pop3


## service: portmap

Open Network Computing Remote Procedure Call - Port Mapper
:   Example:

    ~~~~
      server portmap accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/111 tcp/111

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-portmap]

[WIKI-portmap]: http://en.wikipedia.org/wiki/Portmap


## service: postgres

PostgreSQL
:   Example:

    ~~~~
      server postgres accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5432

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-postgres]

[WIKI-postgres]: http://en.wikipedia.org/wiki/Postgres


## service: pptp

Point-to-Point Tunneling Protocol
:   Example:

    ~~~~
      server pptp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1723

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_pptp [CONFIG_NF_CONNTRACK_PPTP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_PPTP.html)
    * nf_conntrack_proto_gre [CONFIG_NF_CT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_CT_PROTO_GRE.html)

    Netfilter NAT Modules

    * nf_nat_pptp [CONFIG_NF_NAT_PPTP](http://cateee.net/lkddb/web-lkddb/NF_NAT_PPTP.html)
    * nf_nat_proto_gre [CONFIG_NF_NAT_PROTO_GRE](http://cateee.net/lkddb/web-lkddb/NF_NAT_PROTO_GRE.html)

    Links

    * [Wikipedia][WIKI-pptp]

[WIKI-pptp]: http://en.wikipedia.org/wiki/Pptp


## service: privoxy

Privacy Proxy
:   Example:

    ~~~~
      server privoxy accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/8118

    Client Ports:

    * default

    Links

    * [Homepage][HOME-privoxy]

[HOME-privoxy]: http://www.privoxy.org/


## service: radius

Remote Authentication Dial In User Service (RADIUS)
:   Example:

    ~~~~
      server radius accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1812 udp/1813

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-radius]

[WIKI-radius]: http://en.wikipedia.org/wiki/RADIUS


## service: radiusold

Remote Authentication Dial In User Service (RADIUS)
:   Example:

    ~~~~
      server radiusold accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1645 udp/1646

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-radiusold]

[WIKI-radiusold]: http://en.wikipedia.org/wiki/RADIUS


## service: radiusoldproxy

Remote Authentication Dial In User Service (RADIUS)
:   Example:

    ~~~~
      server radiusoldproxy accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1647

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-radiusoldproxy]

[WIKI-radiusoldproxy]: http://en.wikipedia.org/wiki/RADIUS


## service: radiusproxy

Remote Authentication Dial In User Service (RADIUS)
:   Example:

    ~~~~
      server radiusproxy accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1814

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-radiusproxy]

[WIKI-radiusproxy]: http://en.wikipedia.org/wiki/RADIUS


## service: rdp

Remote Desktop Protocol
:   Example:

    ~~~~
      server rdp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3389

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-rdp]

    Notes

    > Remote Desktop Protocol is also known also as
    > Terminal Services.
[WIKI-rdp]: http://en.wikipedia.org/wiki/Remote_Desktop_Protocol


## service: rndc

Remote Name Daemon Control
:   Example:

    ~~~~
      server rndc accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/953

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-rndc]

[WIKI-rndc]: http://en.wikipedia.org/wiki/Rndc


## service: rsync

rsync protocol
:   Example:

    ~~~~
      server rsync accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/873 udp/873

    Client Ports:

    * default

    Links

    * [Homepage][HOME-rsync]
    * [Wikipedia][WIKI-rsync]

[HOME-rsync]: http://rsync.samba.org/
[WIKI-rsync]: http://en.wikipedia.org/wiki/Rsync


## service: rtp

Real-time Transport Protocol
:   Example:

    ~~~~
      server rtp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/10000:20000

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-rtp]

    Notes

    > RTP ports are generally all the UDP ports.
    > This definition narrows down RTP ports to UDP 10000 to 20000.
[WIKI-rtp]: http://en.wikipedia.org/wiki/Real-time_Transport_Protocol


## service: samba

Samba
:   Example:

    ~~~~
      server samba accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * default

    Links

    * [Homepage][HOME-samba]
    * [Wikipedia][WIKI-samba]

    Notes

    > The samba service automatically sets all the rules for
    > [netbios_ns][keyword-service-netbios_ns],
    > [netbios_dgm][keyword-service-netbios_dgm],
    > [netbios_ssn][keyword-service-netbios_ssn] and
    > [microsoft_ds][keyword-service-microsoft_ds].
    > 
    > Please refer to the notes of the above services for more
    > information.
    > 
    > NETBIOS initiates based on the broadcast address of an
    > interface (request goes to broadcast address) but the server
    > responds from its own IP address. This makes the
    > "server samba accept" statement drop the server reply,
    > because of the way the iptables connection tracker works.
    > 
    > This service definition includes a hack, that allows a
    > Linux samba server to respond correctly in such situations,
    > by allowing new outgoing connections from the well known
    > [netbios_ns][keyword-service-netbios_ns] port to the clients
    > high ports.
    > 
    > However, for clients and routers this hack is not applied
    > because it would open all unprivileged ports to the samba
    > server. The only solution to overcome the problem in such
    > cases (routers or clients) is to build a trust relationship
    > between the samba servers and clients.
[HOME-samba]: http://www.samba.org/
[WIKI-samba]: http://en.wikipedia.org/wiki/Samba_(software)


## service: sane

SANE Scanner service
:   Service Type:

    * simple

    Server Ports:

    * tcp/6566

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_sane [CONFIG_NF_CONNTRACK_SANE](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_SANE.html)

    Netfilter NAT Modules

    * N/A

    Links

    * [Homepage][HOME-sane]

[HOME-sane]: http://www.sane-project.org/


## service: sip

Session Initiation Protocol
:   Example:

    ~~~~
      server sip accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5060 udp/5060

    Client Ports:

    * 5060 default

    Netfilter Modules

    * nf_conntrack_sip [CONFIG_NF_CONNTRACK_SIP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_SIP.html)

    Netfilter NAT Modules

    * nf_nat_sip [CONFIG_NF_NAT_SIP](http://cateee.net/lkddb/web-lkddb/NF_NAT_SIP.html)

    Links

    * [Wikipedia][WIKI-sip]

    Notes

    > [SIP](http://www.voip-info.org/wiki/view/SIP) is an IETF
    > standard protocol (RFC 2543) for initiating interactive user
    > sessions involving multimedia elements such as video, voice,
    > chat, gaming, etc. SIP works in the application layer of
    > the OSI communications model.
[WIKI-sip]: http://en.wikipedia.org/wiki/Session_Initiation_Protocol


## service: smtp

Simple Mail Transport Protocol
:   Example:

    ~~~~
      server smtp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/25

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-smtp]

[WIKI-smtp]: http://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol


## service: smtps

Secure Simple Mail Transport Protocol
:   Example:

    ~~~~
      server smtps accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/465

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-smtps]

[WIKI-smtps]: http://en.wikipedia.org/wiki/SMTPS


## service: snmp

Simple Network Management Protocol
:   Example:

    ~~~~
      server snmp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/161

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-snmp]

[WIKI-snmp]: http://en.wikipedia.org/wiki/Simple_Network_Management_Protocol


## service: snmptrap

SNMP Trap
:   Example:

    ~~~~
      server snmptrap accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/162

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-snmptrap]

    Notes

    > An SNMP trap is a notification from an agent to a manager.
[WIKI-snmptrap]: http://en.wikipedia.org/wiki/Simple_Network_Management_Protocol#Trap


## service: socks

SOCKet Secure
:   Example:

    ~~~~
      server socks accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1080 udp/1080

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-socks]

    Notes

    > See also [RFC 1928](http://www.ietf.org/rfc/rfc1928.txt).
[WIKI-socks]: http://en.wikipedia.org/wiki/SOCKS


## service: squid

Squid Web Cache
:   Example:

    ~~~~
      server squid accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/3128

    Client Ports:

    * default

    Links

    * [Homepage][HOME-squid]
    * [Wikipedia][WIKI-squid]

[HOME-squid]: http://www.squid-cache.org/
[WIKI-squid]: http://en.wikipedia.org/wiki/Squid_(software)


## service: ssh

Secure Shell Protocol
:   Example:

    ~~~~
      server ssh accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/22

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-ssh]

[WIKI-ssh]: http://en.wikipedia.org/wiki/Secure_Shell


## service: stun

Session Traversal Utilities for NAT
:   Example:

    ~~~~
      server stun accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/3478 udp/3479

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-stun]

    Notes

    > [STUN](http://www.voip-info.org/wiki/view/STUN)
    > is a protocol for assisting devices behind a NAT firewall or
    > router with their packet routing.
[WIKI-stun]: http://en.wikipedia.org/wiki/STUN


## service: submission

SMTP over SSL/TLS submission
:   Example:

    ~~~~
      server submission accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/587

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-submission]

    Notes

    > Submission is essentially normal SMTP with an SSL/TLS
    > negotiation.
[WIKI-submission]: http://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol


## service: sunrpc

Open Network Computing Remote Procedure Call - Port Mapper
:   Alias for [portmap][keyword-service-portmap]


## service: swat

Samba Web Administration Tool
:   Example:

    ~~~~
      server swat accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/901

    Client Ports:

    * default

    Links

    * [Homepage][HOME-swat]

[HOME-swat]: http://www.samba.org/samba/docs/man/Samba-HOWTO-Collection/SWAT.html


## service: syslog

Syslog Remote Logging Protocol
:   Example:

    ~~~~
      server syslog accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/514

    Client Ports:

    * 514 default

    Links

    * [Wikipedia][WIKI-syslog]

[WIKI-syslog]: http://en.wikipedia.org/wiki/Syslog


## service: telnet

Telnet
:   Example:

    ~~~~
      server telnet accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/23

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-telnet]

[WIKI-telnet]: http://en.wikipedia.org/wiki/Telnet


## service: tftp

Trivial File Transfer Protocol
:   Example:

    ~~~~
      server tftp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/69

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_tftp [CONFIG_NF_CONNTRACK_TFTP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_TFTP.html)

    Netfilter NAT Modules

    * nf_nat_tftp [CONFIG_NF_NAT_TFTP](http://cateee.net/lkddb/web-lkddb/NF_NAT_TFTP.html)

    Links

    * [Wikipedia][WIKI-tftp]

[WIKI-tftp]: http://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol


## service: time

Time Protocol
:   Example:

    ~~~~
      server time accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/37 udp/37

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-time]

[WIKI-time]: http://en.wikipedia.org/wiki/Time_Protocol


## service: timestamp

ICMP Timestamp
:   Example:

    ~~~~
      server timestamp accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * N/A

    Client Ports:

    * N/A

    Links

    * [Wikipedia][WIKI-timestamp]

    Notes

    > This services matches requests of protocol ICMP and type
    > timestamp-request (TYPE=13) and their replies of type
    > timestamp-reply (TYPE=14).
    > 
    > The timestamp service is stateful.
[WIKI-timestamp]: http://en.wikipedia.org/wiki/Internet_Control_Message_Protocol#Timestamp


## service: tomcat

HTTP alternate port
:   Alias for [httpalt][keyword-service-httpalt]


## service: upnp

Universal Plug and Play
:   Example:

    ~~~~
      server upnp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/1900 tcp/2869

    Client Ports:

    * default

    Links

    * [Homepage][HOME-upnp]
    * [Wikipedia][WIKI-upnp]

    Notes

    > For a Linux implementation see:
    > [Linux IGD](http://linux-igd.sourceforge.net/).
[HOME-upnp]: http://upnp.sourceforge.net/
[WIKI-upnp]: http://en.wikipedia.org/wiki/Universal_Plug_and_Play


## service: uucp

Unix-to-Unix Copy
:   Example:

    ~~~~
      server uucp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/540

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-uucp]

[WIKI-uucp]: http://en.wikipedia.org/wiki/UUCP


## service: vmware

vmware
:   Example:

    ~~~~
      server vmware accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/902

    Client Ports:

    * default

    Notes

    > Used from VMWare 1 and up. See the
    > [VMWare KnowledgeBase](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1012382).

## service: vmwareauth

vmwareauth
:   Example:

    ~~~~
      server vmwareauth accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/903

    Client Ports:

    * default

    Notes

    > Used from VMWare 1 and up. See the
    > [VMWare KnowledgeBase](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1012382).

## service: vmwareweb

vmwareweb
:   Example:

    ~~~~
      server vmwareweb accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/8222 tcp/8333

    Client Ports:

    * default

    Notes

    > Used from VMWare 2 and up. See
    > [VMWare Server 2.0 release notes](http://www.vmware.com/support/server2/doc/releasenotes_vmserver2.html)
    > and the
    > [VMWare KnowledgeBase](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1012382).

## service: vnc

Virtual Network Computing
:   Example:

    ~~~~
      server vnc accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/5900:5903

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-vnc]

    Notes

    > VNC is a graphical desktop sharing protocol.
[WIKI-vnc]: http://en.wikipedia.org/wiki/Virtual_Network_Computing


## service: webcache

HTTP alternate port
:   Alias for [httpalt][keyword-service-httpalt]


## service: webmin

Webmin Administration System
:   Example:

    ~~~~
      server webmin accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/10000

    Client Ports:

    * default

    Links

    * [Homepage][HOME-webmin]

[HOME-webmin]: http://www.webmin.com/


## service: whois

WHOIS Protocol
:   Example:

    ~~~~
      server whois accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/43

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-whois]

[WIKI-whois]: http://en.wikipedia.org/wiki/Whois


## service: xbox

Xbox Live
:   Example:

    ~~~~
      client xbox accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * many

    Client Ports:

    * default

    Notes

    > Definition for the Xbox live service.
    > 
    > See program source for contributor details.

## service: xdmcp

X Display Manager Control Protocol
:   Example:

    ~~~~
      server xdmcp accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/177

    Client Ports:

    * default

    Links

    * [Wikipedia][WIKI-xdmcp]

    Notes

    > See [Gnome Display Manager](http://www.jirka.org/gdm-documentation/x70.html)
    > for a discussion about XDMCP and firewalls (Gnome Display
    > Manager is a replacement for XDM).
[WIKI-xdmcp]: http://en.wikipedia.org/wiki/X_display_manager_(program_type)#X_Display_Manager_Control_Protocol

