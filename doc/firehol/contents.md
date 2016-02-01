% FireHOL Reference
% Copyright (c) 2004,2013-2015 Costa Tsaousis <costa@firehol.org>; Copyright (c) 2012-2015 Phil Whineray <phil@firehol.org>
% Version VERSION (Built DATE)

\newpage

<!--
  This file is processed to include inline the individual pages
  single-page HTML and PDF. It is used as-is as a contents page
  for multi-page formats.
  -->

The latest version of this manual is available online as a
[PDF](http://firehol.org/firehol-manual.pdf), as
[single page HTML](http://firehol.org/firehol-manual.html)
and also as
[multiple pages within the website](http://firehol.org/firehol-manual/).

# FireHOL Reference

* [Introduction](introduction.md) <!-- include introduction.md -->

# Setting up and running FireHOL

FireHOL is started and stopped using the [firehol][firehol(1)] script.
The default firewall configuration is to be found in
[/etc/firehol/firehol.conf][firehol.conf(5)], with some behaviours
governed by variables in
[/etc/firehol/firehol-defaults.conf][firehol-variables(5)].

# Primary commands

These are the primary packet filtering building blocks. Below each of
these, sub-commands can be added.

<!-- INSERT TABLE primary -->

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
command                                                   4/6/46                  forbidden params                          description                                                                                                                                           
------------------------------------------------------  ------------------------  ----------------------------------------  ------------------------------------------------------------------------------------------------------------------------------------------------------
[interface][keyword-firehol-interface]                  Y                         inface outface physout                    Define packet filtering blocks, protecting the firewall host itself.                                                                                  

[router][keyword-firehol-router]                        Y                         -                                         Define packet filtering blocks, protecting other hosts from routed traffic.                                                                           
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Sub-commands

A rule in an `interface` or `router` definition typically consists of
a subcommand to apply to a [service][firehol-services(5)] using one of
the standard [actions][firehol-actions(5)] provided it matches certain
[optional rule parameters][firehol-params(5)]. e.g.

~~~~{.firehol}
server ssh accept src 10.0.0.0/8
~~~~

The following sub-commands can be used below **primary commands** to form
rules.

<!-- INSERT TABLE subcommand -->

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
command                                                   4/6/46                  forbidden params                          description                                                                                                                                           
------------------------------------------------------  ------------------------  ----------------------------------------  ------------------------------------------------------------------------------------------------------------------------------------------------------
[client][keyword-firehol-client]                        Y                         sport dport                               Allow access to a client running on the `interface` or the protected `router` hosts.                                                                  

[group][keyword-firehol-group]                          Y                         -                                         Define groups of commands that share optional rule parameters. Groups can be nested.                                                                  

[iptables ip6tables][keyword-firehol-iptables]          N                         *all forbidden*                           A wrapper for the system iptables command, to add custom iptables statements to a FireHOL firewall.                                                   

[masquerade][keyword-firehol-masquerade]                Y                         inface outface                            Change the source IP of packets leaving `outface`, with the IP of the interface they are using to leave.                                              

[policy][keyword-firehol-policy]                        N                         *all forbidden*                           Define the action to be applied on packets not matched by any `server` or `client` statements in the `interface` or `router`.                         

[protection][keyword-firehol-protection]                N                         *all forbidden*                           Examine incoming packets per `interface` or `router` and filter out bad packets or limit request frequency.                                           

[server][keyword-firehol-server]                        Y                         sport dport                               Allow access to a server running on the `interface` or the protected `router` hosts.                                                                  

[tcpmss][keyword-firehol-tcpmss]                        Y                         *all forbidden*                           Set the MSS (Maximum Segment Size) of TCP SYN packets routed through the firewall.                                                                    
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Helper commands

The following commands are generally used to set things up before the
first **primary command**. Some can be used below an `interface` or
`router` and also appear in the [subcommands](#sub-commands) table.

<!-- INSERT TABLE helper -->

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
command                                                   4/6/46                  forbidden params                          description                                                                                                                                           
------------------------------------------------------  ------------------------  ----------------------------------------  ------------------------------------------------------------------------------------------------------------------------------------------------------
[action][keyword-firehol-action]                        Y                         -                                         Define new actions that can differentiate the final action based on rules. `action` can be used to define traps.                                      

[blacklist][keyword-firehol-blacklist]                  Y                         -                                         Drop matching packets globally.                                                                                                                       

[classify][keyword-firehol-classify]                    Y                         -                                         Put matching traffic into the specified traffic shaping class.                                                                                        

[connmark][keyword-firehol-connmark]                    Y                         -                                         Set a stateful mark from the `connmark` group.                                                                                                        

[dscp][keyword-firehol-dscp-helper]                     Y                         -                                         Set the DSCP field of packets.                                                                                                                        

[ipset][keyword-firehol-ipset]                          4/6                       *all forbidden*                           Define ipsets. A wrapper for the system ipset command to add ipsets to a FireHOL firewall.                                                            

[iptables ip6tables][keyword-firehol-ip6tables]         N                         *all forbidden*                           A wrapper for the system iptables command, to add custom iptables statements to a FireHOL firewall.                                                   

[iptrap][keyword-firehol-iptrap]                        4/6                       -                                         Dynamically put IP addresses in an ipset.                                                                                                             

[mac][keyword-firehol-mac-helper]                       Y                         *all forbidden*                           Restricts an IP to a particular MAC address.                                                                                                          

[mark][keyword-firehol-mark-helper]                     Y                         -                                         Set a stateful mark from the `usermark` group.                                                                                                        

[masquerade][keyword-firehol-masquerade]                Y                         -                                         Change the source IP of packets leaving `outface`, with the IP of the interface they are using to leave.                                              

[dnat][keyword-firehol-dnat]                            Y                         -                                         Change the destination IP or port of packets received, to fixed values or fixed ranges. `dnat` can be used to implement load balancers.               

[snat][keyword-firehol-snat]                            Y                         -                                         Change the source IP or port of packets leaving, to fixed values or fixed ranges.                                                                     

[redirect][keyword-firehol-redirect-helper]             Y                         -                                         Redirect packets to the firewall host, possibly changing the destination port. Can support load balancers if multiple daemons run on localhost.       

[transparent_proxy][keyword-firehol-transparent_proxy]  Y                         *see notes*                               Set up a transparent TCP, HTTP or squid proxy.                                                                                                        

[synproxy][keyword-firehol-synproxy]                    Y                         -                                         Configure synproxy.                                                                                                                                   

[tcpmss][keyword-firehol-tcpmss]                        Y                         *all forbidden*                           Set the MSS (Maximum Segment Size) of TCP SYN packets routed through the firewall.                                                                    

[tos][keyword-firehol-tos-helper]                       Y                         -                                         Set the Type of Service (TOS) of packets.                                                                                                             

[tosfix][keyword-firehol-tosfix]                        Y                         *all forbidden*                           Apply suggested TOS values to packets.                                                                                                                

[version][keyword-firehol-version]                      N                         *all forbidden*                           Specify a version number for the configuration file.                                                                                                  
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Manual Pages in Alphabetical Order

* [firehol(1)](firehol.1.md) <!-- include firehol.1.md -->
* [firehol.conf(5)](firehol-conf.5.md) <!-- include firehol-conf.5.md -->
* [firehol-action(5)](firehol-action.5.md) <!-- include firehol-action.5.md -->
* [firehol-actions(5)](firehol-actions.5.md) <!-- include firehol-actions.5.md -->
* [firehol-blacklist(5)](firehol-blacklist.5.md) <!-- include firehol-blacklist.5.md -->
* [firehol-classify(5)](firehol-classify.5.md) <!-- include firehol-classify.5.md -->
* [firehol-client(5)](firehol-client.5.md) <!-- include firehol-client.5.md -->
* [firehol-connmark(5)](firehol-connmark.5.md) <!-- include firehol-connmark.5.md -->
* [firehol-dscp(5)](firehol-dscp.5.md) <!-- include firehol-dscp.5.md -->
* [firehol-group(5)](firehol-group.5.md) <!-- include firehol-group.5.md -->
* [firehol-interface(5)](firehol-interface.5.md) <!-- include firehol-interface.5.md -->
* [firehol-ipset(5)](firehol-ipset.5.md) <!-- include firehol-ipset.5.md -->
* [firehol-iptables(5)](firehol-iptables.5.md) <!-- include firehol-iptables.5.md -->
* [firehol-iptrap(5)](firehol-iptrap.5.md) <!-- include firehol-iptrap.5.md -->
* [firehol-mac(5)](firehol-mac.5.md) <!-- include firehol-mac.5.md -->
* [firehol-mark(5)](firehol-mark.5.md) <!-- include firehol-mark.5.md -->
* [firehol-masquerade(5)](firehol-masquerade.5.md) <!-- include firehol-masquerade.5.md -->
* [firehol-modifiers(5)](firehol-modifiers.5.md) <!-- include firehol-modifiers.5.md -->
* [firehol-nat(5)](firehol-nat.5.md) <!-- include firehol-nat.5.md -->
* [firehol-params(5)](firehol-params.5.md) <!-- include firehol-params.5.md -->
* [firehol-policy(5)](firehol-policy.5.md) <!-- include firehol-policy.5.md -->
* [firehol-protection(5)](firehol-protection.5.md) <!-- include firehol-protection.5.md -->
* [firehol-proxy(5)](firehol-proxy.5.md) <!-- include firehol-proxy.5.md -->
* [firehol-router(5)](firehol-router.5.md) <!-- include firehol-router.5.md -->
* [firehol-server(5)](firehol-server.5.md) <!-- include firehol-server.5.md -->
* [firehol-services(5)](firehol-services.5.md) <!-- include firehol-services.5.md -->
* [firehol-synproxy(5)](firehol-synproxy.5.md) <!-- include firehol-synproxy.5.md -->
* [firehol-tcpmss(5)](firehol-tcpmss.5.md) <!-- include firehol-tcpmss.5.md -->
* [firehol-tos(5)](firehol-tos.5.md) <!-- include firehol-tos.5.md -->
* [firehol-tosfix(5)](firehol-tosfix.5.md) <!-- include firehol-tosfix.5.md -->
* [firehol-variables(5)](firehol-variables.5.md) <!-- include firehol-variables.5.md -->
* [firehol-version(5)](firehol-version.5.md) <!-- include firehol-version.5.md -->
