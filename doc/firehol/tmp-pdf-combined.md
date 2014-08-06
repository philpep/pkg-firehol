% FireHOL Reference
% Copyright (c) 2004,2013-2014 Costa Tsaousis <costa@tsaousis.org>; Copyright (c) 2012-2014 Phil Whineray <phil@sanewall.org>
% Version 2.0.0-rc.1 (Built 02 Aug 2014)

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


Who should read this manual
---------------------------

This is a reference guide with specific detailed information on
commands and configuration syntax for the FireHOL tool.
The reference is unlikely to be suitable for newcomers to the tools,
except as a means to look up more information on a particular command.

For tutorials and guides to using FireHOL and FireQOS, please visit
the [website](http://firehol.org/).

Where to get help
-----------------

The [FireHOL website](http://firehol.org/).

The [mailing lists and
archives](http://lists.firehol.org/mailman/listinfo).

The package comes with a complete set of manpages, a README and a brief
INSTALL guide.

Installation
------------

You can download tar-file releases by visiting the [FireHOL website
download area](http://firehol.org/download/).

Unpack and change directory with:

    tar xfz firehol-version.tar.gz
    cd firehol-version
          

Options for the configure program can be seen in the INSTALL file and by
running:

    ./configure --help
          

To build and install taking the default options:

    ./configure && make && sudo make install
          

Alternatively, just copy the `sbin/firehol.in` file to where you want it.
All of the common SysVInit command line arguments are recognised which
makes it easy to deploy the script as a startup service.

Packages are available for most distributions and you can use your
distribution's standard commands (e.g. aptitude, yum, etc.) to install
these.

> **Note**
>
> Distributions do not always offer the latest version. You can see what
> the latest release is on the [FireHOL website](http://firehol.org/).

Licence
-------

This manual is licensed under the same terms as the FireHOL package, the
GNU GPL v2 or later.

This program is free software; you can redistribute it and/or modify it
under the terms of the GNU General Public License as published by the
Free Software Foundation; either version 2 of the License, or (at your
option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
Public License for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation, Inc.,
59 Temple Place, Suite 330, Boston, MA 02111-1307 USA

\newpage

## Running and Configuring FireHOL


### firehol(1)

#### NAME
 

firehol - an easy to use but powerful iptables stateful firewall

#### SYNOPSIS
 

firehol

sudo -E firehol panic [ *IP* ]

firehol *command* [ -- *conf-arg*... ]

firehol *CONFIGFILE* \[start|debug|try\] [-- *conf-arg*... ]

#### DESCRIPTION
 

Running `firehol` invokes iptables(8) to manipulate your firewall.

Run without any arguments, `firehol` will present some help on usage.

When given *CONFIGFILE*, `firehol` will use the named file instead of
`/etc/firehol/firehol.conf` as its configuration. If no *command* is
given, `firehol` assumes `try`.

It is possible to pass arguments for use by the configuration file
separating any conf-arg values from the rest of the arguments with `--`.
The arguments are accessible in the configuration using standard
bash(1) syntax e.g. \$1, \$2, etc.

##### PANIC
 

To block all communication, invoke `firehol` with the `panic` command.

FireHOL removes all rules from the running firewall and then DROPs all
traffic on all iptables(8) tables (mangle, nat, filter) and pre-defined
chains (PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING).

DROPing is not done by changing the default policy to DROP, but by
adding one rule per table/chain to drop all traffic. This allows systems
which do not reset all the chains to ACCEPT when starting to function
correctly.

When activating panic mode, FireHOL checks for the existence of the
SSH\_CLIENT shell environment variable, which is set by ssh(1). If it
finds this, then panic mode will allow the established SSH connection
specified in this variable to operate.

> **Note**
>
> In order for FireHOL to see the environment variable you must ensure
> that it is preserved. For sudo(8) use the `-E` and for su(1) omit the
> `-` (minus sign).

If SSH\_CLIENT is not set, the *IP* after the panic argument allows you
to give an IP address for which all established connections between the
IP address and the host in panic will be allowed to continue.

#### COMMANDS
 

start; restart
:   Activates the firewall using `/etc/firehol/firehol.conf`.

    Use of the term `restart` is allowed for compatibility with common
    init implementations.

try
:   Activates the firewall, waiting for the user to type the word
    `commit`. If this word is not typed within 30 seconds, the previous
    firewall is restored.

stop
:   Stops a running iptables(8) firewall by clearing all of the tables and
    chains and setting the default policies to ACCEPT. This will allow
    all traffic to pass unchecked.

condrestart
:   Restarts the FireHOL firewall only if it is already active. This is
    the generally expected behaviour (but opposite to FireHOL prior to
    2.0.0-pre4).

status
:   Shows the running firewall, using `/sbin/iptables -nxvL | less`.

save
:   Start the firewall and then save it using iptables-save(8) to
    the location given by FIREHOL\_AUTOSAVE. See
    [firehol-variables(5)][] for more information.

    The required kernel modules are saved to an executable shell script
    `/var/spool/firehol/last_save_modules.sh`, which can be called
    during boot if a firewall is to be restored.

    > **Note**
    >
    > External changes may cause a firewall restored after a reboot to
    > not work as intended where starting the firewall with FireHOL will
    > work.
    >
    > This is because as part of starting a firewall, FireHOL checks
    > some changeable values. For instance the current kernel
    > configuration is checked (for client port ranges), and RPC servers
    > are queried (to allow correct functioning of the NFS service).

<a id="debug"></a>debug
:   Parses the configuration file but instead of activating it, FireHOL
    shows the generated iptables(8) statements.

<a id="explain"></a>explain
:   Enters an interactive mode where FireHOL accepts normal
    configuration commands and presents the generated iptables(8) commands
    for each of them, together with some reasoning for its purpose.

    Additionally, FireHOL automatically generates a configuration script
    based on the successful commands given.

    Some extra commands are available in `explain` mode.

    help
    :   Present some help

    show
    :   Present the generated configuration

    quit
    :   Exit interactive mode and quit

<a id="helpme-wizard"></a>helpme; wizard
:   Tries to guess the FireHOL configuration needed for the current
    machine.

    FireHOL will not stop or alter the running firewall. The
    configuration file is given in the standard output of firehol,
    thus `firehol helpme > /tmp/firehol.conf` will produce the output in
    `/tmp/firehol.conf`.

    The generated FireHOL configuration *must* be edited before use on
    your systems. You are required to take a number of decisions; the
    comments in the generated file will instruct you in the choices you
    must make.

#### FILES
 

`/etc/firehol/firehol.conf`

#### SEE ALSO
 

* [firehol.conf(5)][] - FireHOL configuration
* [firehol-variables(5)][] - control variables
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol.conf(5)

#### NAME
 

firehol.conf - FireHOL configuration

<!--
extra-manpage: firehol.conf.5
  -->

#### DESCRIPTION
 


`/etc/firehol/firehol.conf` is the default configuration file for
[firehol(1)][]. It defines the stateful firewall that will
be produced.

A configuration file starts with an optional version indicator which
looks like this:

    version 5

See [firehol-version(1)][keyword-firehol-version] for full details.

A configuration file contains one or more `interface` definitions, which
look like this:

~~~~
 interface eth0 lan
   client all accept # This host can access any remote service
   server ssh accept # Remote hosts can access SSH on local server
   # ...
~~~~
        

The above definition has name "lan" and specifies a network interface
(eth0). A definition may contain zero or more subcommands. See
[firehol-interface(5)][keyword-firehol-interface] for full details.

By default FireHOL will try to create both IPv4 and IPv6 rules for each
interface. To make this explicit or restrict which rules are created
write `both interface`, `ipv4 interface` or `ipv6 interface`.

A configuration file contains zero or more `router` definitions, which
look like this:

~~~~
DMZ_IF=eth0
WAN_IF=eth1
router wan2dmz inface ${WAN_IF} outface ${DMZ_IF}
  route http accept  # Hosts on WAN may access HTTP on hosts in DMZ
  server ssh accept  # Hosts on WAN may access SSH on hosts in DMZ
  client pop3 accept # Hosts in DMZ may access POP3 on hosts on WAN
  # ...
~~~~
        

The above definition has name "wan2dmz" and specifies incoming and
outgoing network interfaces (eth1 and eth0) using variables. A
definition may contain zero or more subcommands. Note that a router is
not required to specify network interfaces to operate on. See
[firehol-router(5)][keyword-firehol-router] for full details.

By default FireHOL will try to create both IPv4 and IPv6 rules for each
router. To make this explicit or restrict which rules are created
write `both router`, `ipv4 router` or `ipv6 router`.

It is simple to add extra service definitions which can then be used in
the same way as those provided as standard. See
[ADDING SERVICES][].

The configuration file is parsed as a bash(1) script, allowing you to
set up and use variables, flow control and external commands.

Special control variables may be set up and used outside of any definition,
see [firehol-variables(5)][] as can the functions in
[CONFIGURATION HELPER COMMANDS][] and
[HELPER COMMANDS][].

#### VARIABLES AVAILABLE
 

The following variables are made available in the FireHOL configuration
file and can be accessed as \${*VARIABLE*}.

UNROUTABLE\_IPS
:   This variable includes the IPs from both PRIVATE\_IPS and
    RESERVED\_IPS. It is useful to restrict traffic on interfaces and
    routers accepting Internet traffic, for example:

        interface eth0 internet src not "${UNROUTABLE_IPS}"
                  

PRIVATE\_IPS
:   This variable includes all the IP addresses defined as Private or
    Test by [RFC 3330](https://tools.ietf.org/html/rfc3330).

    You can override the default values by creating a file called
    `/etc/firehol/PRIVATE_IPS`.

RESERVED\_IPS
:   This variable includes all the IP addresses defined by
    [IANA](http://www.iana.org/) as reserved.

    You can override the default values by creating a file called
    `/etc/firehol/RESERVED_IPS`.

    Now that IPv4 address space has all been allocated there is very
    little reason that this value will need to change in future.

MULTICAST\_IPS
:   This variable includes all the IP addresses defined as Multicast by
    [RFC 3330](https://tools.ietf.org/html/rfc3330).

    You can override the default values by creating a file called
    `/etc/firehol/MULTICAST_IPS`.

#### ADDING SERVICES
 

To define new services you add the appropriate lines before using them
later in the configuration file.

The following are required:

> server\_*myservice*\_ports="*proto*/*sports*"

> client\_*myservice*\_ports="*cports*"

*proto* is anything iptables(8) accepts e.g. "tcp", "udp", "icmp",
including numeric protocol values.

*sports* is the ports the server is listening at. It is a space-separated
list of port numbers, names and ranges (from:to). The keyword `any` will
match any server port.

*cports* is the ports the client may use to initiate a connection. It is a
space-separated list of port numbers, names and ranges (from:to). The
keyword `any` will match any client port. The keyword `default` will
match default client ports. For the local machine (e.g. a `client`
within an `interface`) it resolves to sysctl(8) variable
net.ipv4.ip\_local\_port\_range (or `/proc/sys/net/ipv4/ip_local_port_range`).
For a remote machine (e.g. a client within an interface or anything
in a router) it resolves to the variable DEFAULT\_CLIENT\_PORTS (see
[firehol-variables(5)][]).

The following are optional:

> require\_*myservice*\_modules="*modules*"

> require\_*myservice*\_nat\_modules="*nat-modules*"

The named kernel modules will be loaded when the definition is used. The
NAT modules will only be loaded if FIREHOL\_NAT is non-zero (see
[firehol-variables(5)][]).

For example, for a service named `daftnet` that listens at two ports,
port 1234 TCP and 1234 UDP where the expected client ports are the
default random ports a system may choose, plus the same port numbers the
server listens at, with further dynamic ports requiring kernel modules
to be loaded:

~~~~
    # Setup service
    server_daftnet_ports="tcp/1234 udp/1234"
    client_daftnet_ports="default 1234"
    require_daftnet_modules="ip_conntrack_daftnet"
    require_daftnet_nat_modules="ip_nat_daftnet
    
    interface eth0 lan0
      server daftnet accept
     
    interface eth1 lan1
      client daftnet reject
    
    router lan2lan inface eth0 outface eth1
      route daftnet accept
~~~~

Where multiple ports are provides (as per the example), FireHOL simply
determines all of the combinations of client and server ports and
generates multiple iptables(8) statements to match them.

To create more complex rules, or stateless rules, you will need to
create a bash function prefixed `rules_` e.g. `rules_myservice`. The
best reference is the many such functions in the main firehol(1)
script.

When adding a service which uses modules, or via a custom function, you
may also wish to include the following:

> ALL\_SHOULD\_ALSO\_RUN="${ALL\_SHOULD\_ALSO\_RUN} *myservice*"

which will ensure your service is set-up correctly as part of the `all`
service.

> **Note**
>
> To allow definitions to be shared you can instead create files and
> install them in the `/etc/firehol/services` directory with a `.conf`
> extension.
>
> The first line must read:
>
>     #FHVER: 1:213
>
> 1 is the service definition API version. It will be changed if the API
> is ever modified. The 213 originally referred to a FireHOL 1.x minor
> version but is no longer checked.
>
> FireHOL will refuse to run if the API version does not match the
> expected one.


#### DEFINITIONS
 

 * [firehol-interface(5)][keyword-firehol-interface] - interface definition
 * [firehol-router(5)][keyword-firehol-router] - router definition

#### SUBCOMMANDS
 

 * [firehol-policy(5)][keyword-firehol-policy] - policy command
 * [firehol-protection(5)][keyword-firehol-protection] - protection command
 * [firehol-server(5)][keyword-firehol-server] - server, route commands
 * [firehol-client(5)][keyword-firehol-client] - client command
 * [firehol-group(5)][keyword-firehol-group] - group command

#### HELPER COMMANDS
 

These helpers can be used in `interface` and `router` definitions as
well as before them:

 * [firehol-iptables(5)][keyword-firehol-iptables] - iptables helper
 * [firehol-masquerade(5)][keyword-firehol-masquerade] - masquerade helper

This helper can be used in `router` definitions as well as before any
`router` or `interface`:

 * [firehol-tcpmss(5)][keyword-firehol-tcpmss] - tcpmss helper

#### CONFIGURATION HELPER COMMANDS
 

These helpers should only be used outside of `interface` and `router`
definitions (i.e. before the first interface is defined).

* [firehol-version(5)][keyword-firehol-version] - version config helper
* [firehol-action(5)][keyword-firehol-action] - action config helper
* [firehol-blacklist(5)][keyword-firehol-blacklist] - blacklist config helper
* [firehol-classify(5)][keyword-firehol-classify] - classify config helper
* [firehol-connmark(5)][keyword-firehol-connmark] - connmark config helper
* [firehol-dscp(5)][keyword-firehol-dscp-helper] - dscp config helper
* [firehol-mac(5)][keyword-firehol-mac-helper] - mac config helper
* [firehol-mark(5)][keyword-firehol-mark-helper] - mark config helper
* [firehol-nat(5)][keyword-firehol-nat] - nat, snat, dnat, redirect helpers
* [firehol-proxy(5)][] - transparent proxy/squid helpers
* [firehol-tos(5)][keyword-firehol-tos-helper] - tos config helper
* [firehol-tosfix(5)][keyword-firehol-tosfix] - tosfix config helper

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol-variables(5)][] - control variables
* [firehol-services(5)][] - services list
* [firehol-actions(5)][] - actions for rules
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-variables(5)

#### NAME
 

firehol-variables - control variables for FireHOL

#### SYNOPSIS
 

Defaults:

* DEFAULT\_INTERFACE\_POLICY="DROP"
* DEFAULT\_ROUTER\_POLICY="RETURN"
* UNMATCHED\_INPUT\_POLICY="DROP"
* UNMATCHED\_OUTPUT\_POLICY="DROP"
* UNMATCHED\_FORWARD\_POLICY="DROP"
* FIREHOL\_INPUT\_ACTIVATION\_POLICY="ACCEPT"
* FIREHOL\_OUTPUT\_ACTIVATION\_POLICY="ACCEPT"
* FIREHOL\_FORWARD\_ACTIVATION\_POLICY="ACCEPT"
* FIREHOL\_LOG\_MODE="LOG"
* FIREHOL\_LOG\_LEVEL=*see notes*
* FIREHOL\_LOG\_OPTIONS="--log-level warning"
* FIREHOL\_LOG\_FREQUENCY="1/second"
* FIREHOL\_LOG\_BURST="5"
* FIREHOL\_LOG\_PREFIX=""
* FIREHOL\_DROP\_INVALID="0"
* DEFAULT\_CLIENT\_PORTS="1000:65535"
* FIREHOL\_NAT="0"
* FIREHOL\_ROUTING="0"
* FIREHOL\_AUTOSAVE=*see notes*
* FIREHOL\_AUTOSAVE6=*see notes*
* FIREHOL\_LOAD\_KERNEL\_MODULES="1"
* FIREHOL\_TRUST\_LOOPBACK="1"
* FIREHOL\_DROP\_ORPHAN\_TCP\_ACK\_FIN="0"
* FIREHOL\_DEBUGGING=""
* WAIT\_FOR\_IFACE=""

#### DESCRIPTION
 

There are a number of variables that control the behaviour of FireHOL.

All variables may be set in the main FireHOL configuration file
`/etc/firehol/firehol.conf`.

Variables which affect the runtime but not the created firewall may also
be set as environment variables before running
[firehol(1)][]. These can change the default values but will
be overwritten by values set in the configuration file. If a variable
can be set by an environment variable it is specified below.

FireHOL also sets some variables before processing the configuration
file which you can use as part of your configuration. These are
described in [firehol.conf(5)][].


#### VARIABLES
 


DEFAULT\_INTERFACE\_POLICY
:   This variable controls the default action to be taken on traffic not
    matched by any rule within an interface. It can be overridden using
    [firehol-policy(5)][keyword-firehol-policy].

    Packets that reach the end of an interface without an action of
    return or accept are logged. You can control the frequency of this
    logging by altering FIREHOL\_LOG\_FREQUENCY.

    Example:

    ~~~~

    DEFAULT_INTERFACE_POLICY="REJECT"
    ~~~~

                  

DEFAULT\_ROUTER\_POLICY
:   This variable controls the default action to be taken on traffic not
    matched by any rule within a router. It can be overridden using
    [firehol-policy(5)][keyword-firehol-policy].

    Packets that reach the end of a router without an action of return
    or accept are logged. You can control the frequency of this logging
    by altering FIREHOL\_LOG\_FREQUENCY.

    Example:

    ~~~~

    DEFAULT_ROUTER_POLICY="REJECT"
    ~~~~

                  

UNMATCHED\_{INPUT|OUTPUT|FORWARD}\_POLICY
:   These variables control the default action to be taken on traffic
    not matched by any interface or router definition that was incoming,
    outgoing or for forwarding respectively. Any supported value from
    [firehol-actions(5)][] may be set.

    All packets that reach the end of a chain are logged, regardless of
    these settings. You can control the frequency of this logging by
    altering FIREHOL\_LOG\_FREQUENCY.

    Example:

    ~~~~

    UNMATCHED_INPUT_POLICY="REJECT"
    UNMATCHED_OUTPUT_POLICY="REJECT"
    UNMATCHED_FORWARD_POLICY="REJECT"
    ~~~~
                  

FIREHOL\_{INPUT|OUTPUT|FORWARD}\_ACTIVATION\_POLICY
:   These variables control the default action to be taken on traffic
    during firewall activation for incoming, outgoing and forwarding
    respectively. Acceptable values are `ACCEPT`, `DROP` and `REJECT`.
    They may be set as environment variables.

    FireHOL defaults all values to `ACCEPT` so that your communications
    continue to work uninterrupted.

    If you wish to prevent connections whilst the new firewall is
    activating, set these values to `DROP`. This is important to do if
    you are using `all` or `any` to match traffic; connections
    established during activation will continue even if they would not
    be allowed once the firewall is established.

    Example:

    ~~~~

    FIREHOL_INPUT_ACTIVATION_POLICY="DROP"
    FIREHOL_OUTPUT_ACTIVATION_POLICY="DROP"
    FIREHOL_FORWARD_ACTIVATION_POLICY="DROP"
    ~~~~


FIREHOL\_LOG\_MODE
:   This variable controls method that FireHOL uses for logging.

    Acceptable values are `LOG` (normal syslog) and `ULOG` (netfilter
    ulogd). When `ULOG` is selected, FIREHOL\_LOG\_LEVEL is ignored.

    Example:

    ~~~~

    FIREHOL_LOG_MODE="ULOG"
    ~~~~

    To see the available options run: `/sbin/iptables -j LOG --help` or
    `/sbin/iptables -j ULOG --help`

FIREHOL\_LOG\_LEVEL
:   This variable controls the level at which events will be logged to
    syslog.

    To avoid packet logs appearing on your console you should ensure
    klogd only logs traffic that is more important than that produced by
    FireHOL.

    Use the following option to choose an iptables(8) log level (alpha or
    numeric) which is higher than the `-c` of klogd.

      iptables           klogd     description
      ------------------ --------- ---------------------------------------------
      emerg (0)          0         system is unusable
      alert (1)          1         action must be taken immediately
      crit (2)           2         critical conditions
      error (3)          3         error conditions
      warning (4)        4         warning conditions
      notice (5)         5         normal but significant condition
      info (6)           6         informational
      debug (7)          7         debug-level messages

      : iptables/klogd levels

    > **Note**
    >
    > The default for klogd is generally to log everything (7 and lower)
    > and the default level for iptables(4) is to log as warning (4).

FIREHOL\_LOG\_OPTIONS
:   This variable controls the way in which events will be logged to
    syslog.

    Example:

    ~~~~

    FIREHOL_LOG_OPTIONS="--log-level info \
                         --log-tcp-options --log-ip-options"
    ~~~~

    To see the available options run: `/sbin/iptables -j LOG --help`

FIREHOL\_LOG\_FREQUENCY; FIREHOL\_LOG\_BURST
:   These variables control the frequency that each logging rule will
    write events to syslog. FIREHOL\_LOG\_FREQUENCY is set to the
    maximum average frequency and FIREHOL\_LOG\_BURST specifies the
    maximum initial number.

    Example:

    ~~~~

    FIREHOL_LOG_FREQUENCY="30/minute"
    FIREHOL_LOG_BURST="2"
    ~~~~

    To see the available options run: `/sbin/iptables -m limit --help`

FIREHOL\_LOG\_PREFIX
:   This value is added to the contents of each logged line for easy
    detection of FireHOL lines in the system logs. By default it is
    empty.

    Example:

    ~~~~

    FIREHOL_LOG_PREFIX="FIREHOL:"
    ~~~~

FIREHOL\_DROP\_INVALID
:   If set to 1, this variable causes FireHOL to drop all packets
    matched as `INVALID` in the iptables(8) connection tracker.

    You may be better off using
    [firehol-protection(5)][keyword-firehol-protection] to control
    matching of `INVALID` packets and others on a per-interface
    and per-router basis.

    > **Note**
    >
    > Care must be taken on IPv6 interfaces, since ICMPv6 packets such
    > as Neighbour Discovery are not tracked, meaning they are marked
    > as INVALID.

    Example:

    ~~~~

    FIREHOL_DROP_INVALID="1"
    ~~~~

DEFAULT\_CLIENT\_PORTS
:   This variable controls the port range that is used when a remote
    client is specified. For clients on the local host, FireHOL finds
    the exact client ports by querying the kernel options.

    Example:

    ~~~~

    DEFAULT_CLIENT_PORTS="0:65535"
    ~~~~

FIREHOL\_NAT
:   If set to 1, this variable causes FireHOL to load the NAT kernel
    modules. If you make use of the NAT helper commands, the variable
    will be set to 1 automatically. It may be set as an environment
    variable.

    Example:

    ~~~~

    FIREHOL_NAT="1"
    ~~~~

FIREHOL\_ROUTING
:   If set to 1, this variable causes FireHOL to enable routing in the
    kernel. If you make use of `router` definitions or certain helper
    commands the variable will be set to 1 automatically. It may be set
    as an environment variable.

    Example:

    ~~~~

    FIREHOL_ROUTING="1"
    ~~~~

FIREHOL\_AUTOSAVE; FIREHOL\_AUTOSAVE6
:   These variables specify the file of IPv4/IPv6 rules that will be
    created when [firehol(1)][] is called with the `save`
    argument. It may be set as an environment variable.

    If the variable is not set, a system-specific value is used which
    was defined at configure-time. If no value was chosen then the save
    fails.

    Example:

    ~~~~

    FIREHOL_AUTOSAVE="/tmp/firehol-saved-ipv4.txt"
    FIREHOL_AUTOSAVE6="/tmp/firehol-saved-ipv6.txt"
    ~~~~

FIREHOL\_LOAD\_KERNEL\_MODULES
:   If set to 0, this variable forces FireHOL to not load any kernel
    modules. It is needed only if the kernel has modules statically
    included and in the rare event that FireHOL cannot access the kernel
    configuration. It may be set as an environment variable.

    Example:

    ~~~~

    FIREHOL_LOAD_KERNEL_MODULES="0"
    ~~~~

FIREHOL\_TRUST\_LOOPBACK
:   If set to 0, the loopback device "lo" will not be trusted and you
    can write standard firewall rules for it.

    > **Warning**
    >
    > If you do not set up appropriate rules, local processes will not
    > be able to communicate with each other which can result in serious
    > breakages.

    By default "lo" is trusted and all `INPUT` and `OUTPUT` traffic is
    accepted (forwarding is not included).

    Example:

    ~~~~

    FIREHOL_TRUST_LOOPBACK="0"
    ~~~~

FIREHOL\_DROP\_ORPHAN\_TCP\_ACK\_FIN
:   If set to 1, FireHOL will drop all TCP connections with ACK FIN set
    without logging them.

    In busy environments the iptables(8) connection tracker removes
    connection tracking list entries as soon as it receives a FIN. This
    makes the ACK FIN appear as an invalid packet which will normally be
    logged by FireHOL.

    Example:

    ~~~~

    FIREHOL_DROP_ORPHAN_TCP_ACK_FIN="1"
    ~~~~

FIREHOL\_DEBUGGING
:   If set to a non-empty value, switches on debug output so that it is
    possible to see what processing FireHOL is doing.

    > **Note**
    >
    > This variable can *only* be set as an environment variable, since
    > it is processed before any configuration files are read.

    Example:

    ~~~~

    FIREHOL_DEBUGGING="Y"
    ~~~~

WAIT\_FOR\_IFACE
:   If set to the name of a network device (e.g. eth0), FireHOL will
    wait until the device is up (or until 60 seconds have elapsed)
    before continuing.

    > **Note**
    >
    > This variable can *only* be set as an environment variable, since
    > it determines when the main configuration file will be processed.

    A device does not need to be up in order to have firewall rules
    created for it, so this option should only be used if you have a
    specific need to wait (e.g. the network must be queried to determine
    the hosts or ports which will be firewalled).

    Example:

    ~~~~

    WAIT_FOR_IFACE="eth0"
    ~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-nat(5)][keyword-firehol-nat] - nat, snat, dnat, redirect helpers
* [firehol-actions(5)][] - actions for rules
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-modifiers(5)

#### NAME
 

firehol-modifiers - select IPv4 or IPv6 mode
<!--
extra-manpage: firehol-ipv4.5
extra-manpage: firehol-ipv6.5
-->

#### SYNOPSIS
 

ipv4 *definition-or-command* *argument*...

ipv6 *definition-or-command* *argument*...

#### DESCRIPTION
 

Without a modifier, interface and router definitions and commands that
come before either will be applied to both IPv4 and IPV6. Commands
within an `interface` or `router` assume the same behaviour as the enclosing
definition.

When preceded by a modifier, the command or definition can be made to
apply to IPv4 or IPv6 only. Note that you cannot create an IPv4 only
command within and IPv6 interface or vice-versa.

Examples:

~~~~

 interface eth0 myboth src4 192.0.2.0/24 src6 2001:DB8::/24
   ipv4 server http accept
   ipv6 server http accept

 ipv4 interface eth0 my4only src 192.0.2.0/24
   server http accept

 ipv6 interface eth0 my6only src 2001:DB8::/24
   server http accept
~~~~

Many definitions and commands have explicitly named variants (such as
router4, router6, router46) which can be used as shorthand.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-policy(5)][keyword-firehol-policy] - policy command
* [firehol-protection(5)][keyword-firehol-protection] - protection command
* [firehol-server(5)][keyword-firehol-server] - server, route commands
* [firehol-client(5)][keyword-firehol-client] - client command
* [firehol-group(5)][keyword-firehol-group] - group command
* [firehol-iptables(5)][keyword-firehol-iptables] - iptables helper
* [firehol-masquerade(5)][keyword-firehol-masquerade] - masquerade helper
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

## Definition Commands for FireHOL


### firehol-interface(5)

#### NAME
 

firehol-interface - interface definition

#### SYNOPSIS
 

{ interface | interface46 } *real-interface* *name* *rule-params*

interface4 *real-interface* *name* *rule-params*

interface6 *real-interface* *name* *rule-params*

<!--
extra-manpage: firehol-interface46.5
extra-manpage: firehol-interface4.5
extra-manpage: firehol-interface6.5
  -->

#### DESCRIPTION
 


An `interface` definition creates a firewall for protecting the host on
which the firewall is running.

The default policy is DROP, so that if no subcommands are given, the
firewall will just drop all incoming and outgoing traffic using this
interface.

The behaviour of the defined interface is controlled by adding subcommands
from those listed in [INTERFACE SUBCOMMANDS][].

> **Note**
>
> Forwarded traffic is never matched by the `interface` rules, even if
> it was originally destined for the firewall but was redirected using
> NAT. Any traffic to be passed through the firewall for whatever reason
> must be in a `router` (see [firehol-router(5)][keyword-firehol-router]).

> **Note**
>
> Writing `interface4` is equivalent to writing `ipv4 interface` and
> ensures the defined interface is created only in the IPv4 firewall
> along with any rules within it.
>
> Writing `interface6` is equivalent to writing `ipv6 interface` and
> ensures the defined interface is created only in the IPv6 firewall
> along with any rules within it.
>
> Writing `interface46` is equivalent to writing `both interface` and
> ensures the defined interface is created in both the IPv4 and IPv6
> firewalls. Any rules within it will also be applied to both, unless
> they specify otherwise.

#### PARAMETERS
 

*real-interface*
:   This is the interface name as shown by `ip link show`. Generally
    anything iptables(8) accepts is valid.

    The + (plus sign) after some text will match all interfaces that
    start with this text.

    Multiple interfaces may be specified by enclosing them within
    quotes, delimited by spaces for example:

        interface "eth0 eth1 ppp0" myname
                  

*name*
:   This is a name for this interface. You should use short names (10
    characters maximum) without spaces or other symbols.

    A name should be unique for all FireHOL interface and router
    definitions.

*rule-params*
:   The set of rule parameters to further restrict the traffic that is
    matched to this interface.

    See [firehol-params(5)][] for information on the
    parameters that can be used. Some examples:

    ~~~~

    interface eth0 intranet src 192.0.2.0/24

    interface eth0 internet src not "${UNROUTABLE_IPS}"
    ~~~~
                  

    See [firehol.conf(5)][] for an explanation
    of \${UNROUTABLE\_IPS}.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [firehol-modifiers(5)][] - ipv4/ipv6 selection
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-iptables(5)][keyword-firehol-iptables] - iptables helper
* [firehol-masquerade(5)][keyword-firehol-masquerade] - masquerade helper
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

##### Interface Subcommands
 

* [firehol-policy(5)][keyword-firehol-policy] - policy command
* [firehol-protection(5)][keyword-firehol-protection] - protection command
* [firehol-server(5)][keyword-firehol-server] - server, route commands
* [firehol-client(5)][keyword-firehol-client] - client command
* [firehol-group(5)][keyword-firehol-group] - group command

\newpage

### firehol-router(5)

#### NAME
 

firehol-router - create a router definition

#### SYNOPSIS
 

{ router | router46 } *name* *rule-params*

router4 *name* *rule-params*

router6 *name* *rule-params*

<!--
extra-manpage: firehol-router46.5
extra-manpage: firehol-router4.5
extra-manpage: firehol-router6.5
  -->

#### DESCRIPTION
 

A `router` definition consists of a set of rules for traffic passing
through the host running the firewall.

The default policy for router definitions is RETURN, meaning packets are
not dropped by any particular router. Packets not matched by any router
are dropped at the end of the firewall.

The behaviour of the defined router is controlled by adding subcommands
from those listed in [ROUTER SUBCOMMANDS][].

> **Note**
>
> Writing `router4` is equivalent to writing `ipv4 router` and ensures
> the defined router is created only in the IPv4 firewall along with any
> rules within it.
>
> Writing `router6` is equivalent to writing `ipv6 router` and ensures
> the defined router is created only in the IPv6 firewall along with any
> rules within it.
>
> Writing `router46` is equivalent to writing `both router` and ensures
> the defined router is created in both the IPv4 and IPv6 firewalls. Any
> rules within it will also be applied to both, unless they specify
> otherwise.


#### PARAMETERS
 


*name*
:   This is a name for this router. You should use short names (10
    characters maximum) without spaces or other symbols.

    A name should be unique for all FireHOL interface and router
    definitions.

*rule-params*
:   The set of rule parameters to further restrict the traffic that is
    matched to this router.

    See [firehol-params(5)][] for information on the
    parameters that can be used. Some examples:

        router mylan inface ppp+ outface eth0 src not ${UNROUTABLE_IPS}

        router myrouter
                  

    See [firehol.conf(5)][] for an explanation
    of \${UNROUTABLE\_IPS}.


#### WORKING WITH ROUTERS
 


Routers create stateful iptables(8) rules which match traffic in both
directions.

To match some client or server traffic, the input/output interface or
source/destination of the request must be specified. All
`inface`/`outface` and `src`/`dst` [firehol-params(5)][]
can be given on the router statement (in which case they will be applied
to all subcommands for the router) or just within the subcommands of the
router.

For example, to define a router which matches requests from any PPP
interface and destined for eth0, and on this allowing HTTP servers (on
eth0) to be accessed by clients (from PPP) and SMTP clients (from eth0)
to access any servers (on PPP):

~~~~
router mylan inface ppp+ outface eth0
  server http accept
  client smtp accept
~~~~

> **Note**
>
> The `client` subcommand reverses any optional rule parameters passed
> to the `router`, in this case the `inface` and `outface`.

Equivalently, to define a router which matches all forwarded traffic and
within the the router allow HTTP servers on eth0 to be accessible to PPP
and any SMTP servers on PPP to be accessible from eth0:

~~~~
router mylan
  server http accept inface ppp+ outface eth0
  server smtp accept inface eth0 outface ppp
~~~~
        

> **Note**
>
> In this instance two `server` subcommands are used since there are no
> parameters on the `router` to reverse. Avoid the use of the `client`
> subcommand in routers unless the inputs and outputs are defined as
> part of the `router`.

Any number of routers can be defined and the traffic they match can
overlap. Since the default policy is RETURN, any traffic that is not
matched by any rules in one will proceed to the next, in order, until
none are left.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [firehol-modifiers(5)][] - ipv4/ipv6 selection
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-iptables(5)][keyword-firehol-iptables] - iptables helper
* [firehol-masquerade(5)][keyword-firehol-masquerade] - masquerade helper
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

##### Router Subcommands
 

* [firehol-policy(5)][keyword-firehol-policy] - policy command
* [firehol-protection(5)][keyword-firehol-protection] - protection command
* [firehol-server(5)][keyword-firehol-server] - server, route commands
* [firehol-client(5)][keyword-firehol-client] - client command
* [firehol-group(5)][keyword-firehol-group] - group command
* [firehol-tcpmss(5)][keyword-firehol-tcpmss] - tcpmss helper

\newpage

## Interface/Router Subcommands for FireHOL


### firehol-policy(5)

#### NAME
 

firehol-policy - set default action for an interface or router

#### SYNOPSIS
 

policy *action*

#### DESCRIPTION
 

The `policy` subcommand defines the default policy for an interface or
router.

The *action* can be any of the actions listed in
[firehol-actions(5)][].

> **Note**
>
> Change the default policy of a router only if you understand clearly
> what will be matched by the router statement whose policy is being
> changed.
>
> It is common to define overlapping router definitions. Changing the
> policy to anything other than the default `return` may cause strange
> results for your configuration.

> **Warning**
>
> Do not set a policy to `accept` unless you fully trust all hosts that
> can reach the interface. FireHOL CANNOT be used to create valid "accept by
> default" firewalls.

#### EXAMPLE
 

~~~~

interface eth0 intranet src 192.0.2.0/24
  # I trust this interface absolutely
  policy accept
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-protection(5)

#### NAME
 

firehol-protection - add extra protections to a definition

#### SYNOPSIS
 

protection [reverse] strong [*requests/period* [*burst*]]

protection [reverse] *flood-protection-type* [*requests/period* [*burst*]]

protection [reverse] { bad-packets | *packet-protection-type* }

#### DESCRIPTION
 


The `protection` subcommand sets protection rules on an interface or
router.

Flood protections honour the values *requests/period* and *burst*. They
are used to limit the rate of certain types of traffic.

The default rate FireHOL uses is 100 operations per second with a burst
of 50. Run `iptables -m limit --help` for more information.

The protection type `strong` will switch on all protections (both packet
and flood protections) except `all-floods`. It has aliases `full` and
`all`.

The protection type `bad-packets` will switch on all packet protections
but not flood protections.

You can specify multiple protection types by using multiple `protection`
commands or by using a single command and enclosing the types in quotes.

> **Note**
>
> On a router, protections are normally set up on inface.
>
> The `reverse` option will set up the protections on outface. You must
> use it as the first keyword.


#### PACKET PROTECTION TYPES
 


invalid
:   Drops all incoming invalid packets, as detected INVALID by the
    connection tracker.

    See also FIREHOL\_DROP\_INVALID in
    [firehol-variables(5)][] which allows setting this
    function globally.

fragments
:   Drops all packet fragments.

    This rule will probably never match anything since iptables(8)
    reconstructs all packets automatically before the firewall rules are
    processed whenever connection tracking is running.

new-tcp-w/o-syn
:   Drops all TCP packets that initiate a socket but have not got the
    SYN flag set.

malformed-xmas
:   Drops all TCP packets that have all TCP flags set.

malformed-null
:   Drops all TCP packets that have all TCP flags unset.

malformed-bad
:   Drops all TCP packets that have illegal combinations of TCP flags
    set.


#### FLOOD PROTECTION TYPES
 


icmp-floods [*requests/period* [*burst*]]
:   Allows only a certain amount of ICMP echo requests.

syn-floods [*requests/period* [*burst*]]
:   Allows only a certain amount of new TCP connections.

    Be careful to not set the rate too low as the rule is applied to all
    connections regardless of their final result (rejected, dropped,
    established, etc).

all-floods [*requests/period* [*burst*]]
:   Allows only a certain amount of new connections.

    Be careful to not set the rate too low as the rule is applied to all
    connections regardless of their final result (rejected, dropped,
    established, etc).


#### EXAMPLES
 

~~~~
protection strong

protection "invalid new-tcp-w/o-syn"

protection syn-floods 90/sec 40
~~~~

#### KNOWN ISSUES
 

When using multiple types in a single command, if the quotes are
forgotten, incorrect rules will be generated without warning.

When using multiple types in a single command, FireHOL will silently
ignore any types that come after a group type (`bad-packets`, `strong`
and its aliases). Only use group types on their own line.


#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-server(5)

#### NAME
 

firehol-server - server, route commands: accept requests to a service

#### SYNOPSIS
 

{ server | server46 } *service* *action* *rule-params*

server4 *service* *action* *rule-params*

server6 *service* *action* *rule-params*

{ route | route46 } *service* *action* *rule-params*

route4 *service* *action* *rule-params*

route6 *service* *action* *rule-params*

<!--
extra-manpage: firehol-server46.5
extra-manpage: firehol-server4.5
extra-manpage: firehol-server6.5
extra-manpage: firehol-route46.5
extra-manpage: firehol-route4.5
extra-manpage: firehol-route6.5
  -->

#### DESCRIPTION
 

The `server` subcommand defines a server of a service on an `interface` or
`router`. Any *rule-params* given to a parent interface or router are
inherited by the server.

For FireHOL a server is the destination of a request. Even though this
is more complex for some multi-socket services, to FireHOL a server
always accepts requests.

The `route` subcommand is an alias for `server` which may only be used
in routers.

The *service* parameter is one of the supported service names from
[firehol-services(5)][]. Multiple services may be
specified, space delimited in quotes.

The *action* can be any of the actions listed in
[firehol-actions(5)][].

The *rule-params* define a set of rule parameters to further restrict
the traffic that is matched to this service. See
[firehol-params(5)][] for more details.

> **Note**
>
> Writing `server4` is equivalent to writing `ipv4 server` and ensures
> this subcommand is applied only in the IPv4 firewall rules.
>
> Writing `server6` is equivalent to writing `ipv6 server` and ensures
> this subcommand is applied only in the IPv6 firewall rules.
>
> Writing `server46` is equivalent to writing `both server` and ensures
> this subcommand is applied in both the IPv4 and IPv6 firewall rules;
> it cannot be used as part an interface or router that is IPv4 or IPv6
> only.
>
> The default `server` inherits its behaviour from the enclosing
> interface or router.
>
> The same rules apply to the variations of `route`.


#### EXAMPLES
 

~~~~
server smtp accept

server "smtp pop3" accept

server smtp accept src 192.0.2.1

server smtp accept log "mail packet" src 192.0.2.1
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-modifiers(5)][] - ipv4/ipv6 selection
* [firehol-services(5)][] - services list
* [firehol-actions(5)][] - actions for rules
* [firehol-params(5)][] - optional rule parameters
* [firehol-client(5)][keyword-firehol-client] - client subcommand
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-client(5)

#### NAME
 

firehol-client - client command

#### SYNOPSIS
 

{ client | client46 } *service* *action* [*rule-params*]

client4 *service* *action* [*rule-params*]

client6 *service* *action* [*rule-params*]

<!--
extra-manpage: firehol-client46.5
extra-manpage: firehol-client4.5
extra-manpage: firehol-client6.5
  -->

#### DESCRIPTION
 

The `client` subcommand defines a client of a service on an interface or
router. Any *rule-params* given to a parent interface or router are
inherited by the client, but are reversed.

For FireHOL a client is the source of a request. Even though this is
more complex for some multi-socket services, to FireHOL a client always
initiates the connection.

The *service* parameter is one of the supported service names from
[firehol-services(5)][]. Multiple services may be
specified, space delimited in quotes.

The *action* can be any of the actions listed in
[firehol-actions(5)][].

The *rule-params* define a set of rule parameters to further restrict
the traffic that is matched to this service. See
[firehol-params(5)][] for more details.

> **Note**
>
> Writing `client4` is equivalent to writing `ipv4 client` and ensures
> this subcommand is applied only in the IPv4 firewall rules.
>
> Writing `client6` is equivalent to writing `ipv6 client` and ensures
> this subcommand is applied only in the IPv6 firewall rules.
>
> Writing `client46` is equivalent to writing `both client` and ensures
> this subcommand is applied in both the IPv4 and IPv6 firewall rules;
> it cannot be used as part an interface or router that is IPv4 or IPv6
> only.
>
> The default `client` inherits its behaviour from the enclosing
> interface or router.


#### EXAMPLES
 

~~~~
client smtp accept

client "smtp pop3" accept

client smtp accept src 192.0.2.1

client smtp accept log "mail packet" src 192.0.2.1
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-modifiers(5)][] - ipv4/ipv6 selection
* [firehol-services(5)][] - services list
* [firehol-actions(5)][] - actions for rules
* [firehol-params(5)][] - optional rule parameters
* [firehol-server(5)][keyword-firehol-server] - server subcommand
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-group(5)

#### NAME
 

firehol-group - group commands with common options

#### SYNOPSIS
 

group with *rule-params*

group end

#### DESCRIPTION
 

The `group` command allows you to group together multiple `client` and
`server` commands.

Grouping commands with common options (see
[firehol-params(5)][]) allows the option values
to be checked only once in the generated firewall rather than once per
service, making it more efficient.

Nested groups may be used.

#### EXAMPLES
 

This:

~~~~

interface any world
  client all accept
  server http accept

  # Provide these services to trusted hosts only
  server "ssh telnet" accept src "192.0.2.1 192.0.2.2"
~~~~

can be replaced to produce a more efficient firewall by this:

~~~~

interface any world
  client all accept
  server http accept

  # Provide these services to trusted hosts only
  group with src "192.0.2.1 192.0.2.2"
    server telnet accept
    server ssh accept
  group end
~~~~


#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-params(5)][] - optional rule parameters
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

## Optional Parameters and Actions for FireHOL


### firehol-params(5)

#### NAME
 

firehol-params - optional rule parameters

<!--
extra-manpage: firehol-src.5
extra-manpage: firehol-src4.5
extra-manpage: firehol-src6.5
extra-manpage: firehol-dst.5
extra-manpage: firehol-dst4.5
extra-manpage: firehol-dst6.5
extra-manpage: firehol-srctype.5
extra-manpage: firehol-dsttype.5
extra-manpage: firehol-dport.5
extra-manpage: firehol-sport.5
extra-manpage: firehol-inface.5
extra-manpage: firehol-outface.5
extra-manpage: firehol-physin.5
extra-manpage: firehol-physout.5
extra-manpage: firehol-custom.5
extra-manpage: firehol-log.5
extra-manpage: firehol-loglimit.5
extra-manpage: firehol-proto.5
extra-manpage: firehol-uid.5
extra-manpage: firehol-gid.5
extra-manpage: firehol-mac-param.5
extra-manpage: firehol-mark-param.5
extra-manpage: firehol-tos-param.5
extra-manpage: firehol-dscp-param.5
extra-manpage: firehol-dport.5
extra-manpage: firehol-sport.5
-->

#### SYNOPSIS
 

_Common_

{ src | src4 | src6 } [not] *host*

{ dst | dst4 | dst6 } [not] *host*

srctype [not] *type*

dsttype [not] *type*

proto [not] *protocol*

mac [not] *macaddr*

dscp [not] *value* *class* *classid*

mark [not] *id*

tos [not] *id*

custom "*iptables-options*..."

_Router Only_

inface [not] *interface*

outface [not] *interface*

physin [not] *interface*

physout [not] *interface*

_Interface Only_

uid [not] *user*

gid [not] *group*

_Logging_

log "log text" [level *loglevel*]

loglimit "log text" [level *loglevel*]

_Other_

sport *port*

dport *port*


#### DESCRIPTION
 

Optional rule parameters are accepted by many commands to narrow the
match they make. Not all parameters are accepted by all commands so you
should check the individual commands for exclusions.

All matches are made against the REQUEST. FireHOL automatically sets up
the necessary  stateful rules to deal with replies in the reverse
direction.

Use the keyword `not` to match any value other than the one(s) specified.

The logging parameters are unusual in that they do not affect the match,
they just cause a log message to be emitted. Therefore, the logging
parameters don't support the `not` option.

FireHOL is designed so that if you specify a parameter that is also used
internally by the command then a warning will be issued (and the
internal version will be used).

#### COMMON
 

##### src, dst
 

Use `src` and `dst` to define the source and destination IP addresses of
the request respectively. *host* defines the IP or IPs to be matched.
Examples:

~~~~
server4 smtp accept src not 192.0.2.1
server4 smtp accept dst 198.51.100.1
server4 smtp accept src not 192.0.2.1 dst 198.51.100.1
server6 smtp accept src not 2001:DB8:1::/64
server6 smtp accept dst 2001:DB8:2::/64
server6 smtp accept src not 2001:DB8:1::/64 dst 2001:DB8:2::/64
~~~~

When attempting to create rules for both IPv4 and IPv6 it is generally
easier to use the `src4`, `src6`, `dst4` and `dst6` pairs:

~~~~
server46 smtp accept src4 192.0.2.1 src6 2001:DB8:1::/64
server46 smtp accept dst4 198.51.100.1 dst6 2001:DB8:2::/64
server46 smtp accept dst4 $d4 dst6 $d6 src4 not $d4 src6 not $s6
~~~~

To keep the rules sane, if one of the 4/6 pair specifies `not`, then so
must the other. If you do not want to use both IPv4 and IPv6 addresses,
you must specify the rule as IPv4 or IPv6 only. It is always possible to
write a second IPv4 or IPv6 only rule.

##### srctype, dsttype
 

Use `srctype` or `dsttype` to define the source or destination IP
address type of the request. *type* is the address type category as used
in the kernel's network stack. It can be one of:

UNSPEC
:   an unspecified address (i.e. 0.0.0.0)

UNICAST
:   a unicast address

LOCAL
:   a local address

BROADCAST
:   a broadcast address

ANYCAST
:   an anycast address

MULTICAST
:   a multicast address

BLACKHOLE
:   a blackhole address

UNREACHABLE
:   an unreachable address

PROHIBIT
:   a prohibited address

THROW; NAT; XRESOLVE
:   undocumented

See iptables(8) or run `iptables -m addrtype --help` for more
information. Examples:

    server smtp accept srctype not "UNREACHABLE PROHIBIT"
        

##### proto
 

Use `proto` to match by protocol. The *protocol* can be any accepted by
iptables(8).

##### mac
 

Use `mac` to match by MAC address. The *macaddr* matches to the "remote"
host. In an `interface`, "remote" always means the non-local host. In a
`router`, "remote" refers to the source of requests for `server`s. It
refers to the destination of requests for `client`s. Examples:

~~~~
 # Only allow pop3 requests to the e6 host
 client pop3 accept mac 00:01:01:00:00:e6

 # Only allow hosts other than e7/e8 to access smtp
 server smtp accept mac not "00:01:01:00:00:e7 00:01:01:00:00:e8"
~~~~
        
##### dscp
 

Use `dscp` to match the DSCP field on packets. For details on DSCP
values and classids, see [firehol-dscp(5)][keyword-firehol-dscp-helper].

~~~~
 server smtp accept dscp not "0x20 0x30"
 server smtp accept dscp not class "BE EF"
~~~~
        

##### mark
 

Use `mark` to match marks set on packets. For details on mark ids, see
[firehol-mark(5)][keyword-firehol-mark-helper].

    server smtp accept mark not "20 55"
        

##### tos
 

Use `tos` to match the TOS field on packets. For details on TOS ids, see
[firehol-tos(5)][keyword-firehol-tos-helper].

    server smtp accept tos not "Maximize-Throughput 0x10"
        

##### custom
 

Use `custom` to pass arguments directly to iptables(8). All of the
parameters must be in a single quoted string. To pass an option to
iptables(8) that itself contains a space you need to quote strings in
the usual bash(1) manner. For example:

~~~~
server smtp accept custom "--some-option some-value"
server smtp accept custom "--some-option 'some-value second-value'"
~~~~


#### ROUTER ONLY
 

##### inface, outface
 

Use `inface` and `outface` to define the *interface* via which a request
is received and forwarded respectively. Use the same format as 
[firehol-interface(5)][keyword-firehol-interface].
Examples:

~~~~
server smtp accept inface not eth0
server smtp accept inface not "eth0 eth1"
server smtp accept inface eth0 outface eth1
~~~~

        
##### physin, physout
 

Use `physin` and `physout` to define the physical *interface* via which a
request is received or send in cases where the inface or outface is
known to be a virtual interface; e.g. a bridge. Use the same format as
[firehol-interface(5)][keyword-firehol-interface]. Examples:

    server smtp accept physin not eth0
        

#### INTERFACE ONLY
 

These parameters match information related to information gathered from
the local host. They apply only to outgoing packets and are silently
ignored for incoming requests and requests that will be forwarded.

> **Note**
>
> The Linux kernel infrastructure to match PID/SID and executable names
> with `pid`, `sid` and `cmd` has been removed so these options can no
> longer be used.

##### uid
 

Use `uid` to match the operating system user sending the traffic. The
*user* is a username, uid number or a quoted list of the two.

For example, to limit which users can access POP3 and IMAP by preventing
replies for certain users from being sent:

    client "pop3 imap" accept user not "user1 user2 user3"
        

Similarly, this will allow all requests to reach the server but prevent
replies unless the web server is running as apache:

    server http accept user apache
        
##### gid
 

Use `gid` to match the operating system group sending the traffic. The
*group* is a group name, gid number or a quoted list of the two.


#### LOGGING
 

##### log, loglimit
 

Use `log` or `loglimit` to log matching packets to syslog. Unlike
iptables(8) logging, this is not an action: FireHOL will produce
multiple iptables(8) commands to accomplish both the action for the rule
and the logging.

Logging is controlled using the FIREHOL\_LOG\_OPTIONS and
FIREHOL\_LOG\_LEVEL environment variables - see
[firehol-variables(5)][]. `loglimit`
additionally honours the FIREHOL\_LOG\_FREQUENCY and FIREHOL\_LOG\_BURST
variables.

Specifying `level` (which takes the same values as FIREHOL\_LOG\_LEVEL)
allows you to override the log level for a single rule.


#### LESSER USED PARAMETERS
 

##### dport, sport
 

FireHOL also provides `dport`, `sport` and `limit` which are used
internally and rarely needed within configuration files.

`dport` and `sport` require an argument *port* which can be a name, number,
range (FROM:TO) or a quoted list of ports.

For `dport` *port* specifies the destination port of a request and can be
useful when matching traffic to helper commands (such as nat) where there
is no implicit port.

For `sport` *port* specifies the source port of a request and can be useful
when matching traffic to helper commands (such as nat) where there is no
implicit port.

##### limit
 

`limit` requires the arguments *frequency* and *burst* and will limit the
matching of traffic in both directions.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-server(5)][keyword-firehol-server] - server, route commands
* [firehol-client(5)][keyword-firehol-client] - client command
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-mark(5)][keyword-firehol-mark-helper] - mark config helper
* [firehol-tos(5)][keyword-firehol-tos-helper] - tos config helper
* [firehol-dscp(5)][keyword-firehol-dscp-helper] - dscp config helper
* [firehol-variables(5)][] - control variables
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-actions(5)

#### NAME
 

firehol-actions - actions for rules

<!--
extra-manpage: firehol-accept.5
extra-manpage: firehol-reject.5
extra-manpage: firehol-drop.5
extra-manpage: firehol-deny.5
extra-manpage: firehol-return.5
extra-manpage: firehol-tarpit.5
-->

#### SYNOPSIS
 

accept

accept with limit *requests/period burst* [overflow *action*]

accept with recent *name* *seconds* *hits*

accept with knock *name*

reject [with *message*]

drop | deny

return

tarpit


#### DESCRIPTION
 

These actions are the actions to be taken on traffic that has been
matched by a particular rule.

FireHOL will also pass through any actions that iptables(8) accepts,
however these definitions provide lowercase versions which accept
arguments where appropriate and which could otherwise not be passed
through.

> **Note**
>
> The iptables(8) LOG action is best used through the optional rule
> parameter `log` since the latter can be combined with one of these
> actions (FireHOL will generate multiple firewall rules to make this
> happen). For more information see [log][keyword-firehol-log]
> and [loglimit][keyword-firehol-log].

The following actions are defined:

##### accept
 

`accept` allows the traffic matching the rules to reach its
destination.

For example, to allow SMTP requests and their replies to flow:

    server smtp accept
                    
##### accept with limit *requests/period burst* [overflow *action*]
 

`accept with limit` allows the traffic, with new connections limited
to *requests/period* with a maximum *burst*. Run
`iptables -m limit --help` for more information.

The default `overflow` *action* is to REJECT the excess connections
(DROP would produce timeouts on otherwise valid service clients).

Examples:

~~~~

server smtp accept with limit 10/sec 100

server smtp accept with limit 10/sec 100 overflow drop
~~~~
                    
##### accept with recent *name* *seconds* *hits*
 

`accept with recent` allows the traffic matching the rules to reach
its destination, limited per remote IP to *hits* per *seconds*. Run
`iptables -m recent --help` for more information.

The *name* parameter is used to allow multiple rules to share the
same table of recent IPs.

For example, to allow only 2 connections every 60 seconds per remote
IP, to the smtp server:

    server smtp accept with recent mail 60 2
                  

> **Note**
>
> When a new connection is not allowed, the traffic will continue to
> be matched by the rest of the firewall. In other words, if the
> traffic is not allowed due to the limitations set here, it is not
> dropped, it is just not matched by this rule.

##### accept with knock *name*
 

`accept with knock` allows easy integration with
[knockd](http://www.zeroflux.org/knock/), a server that allows you
to control access to services by sending certain packets to "knock"
on the door, before the door is opened for service.

The *name* is used to build a special chain knock\_\<`name`\> which
contains rules to allow established connections to work. If knockd
has not allowed new connections any traffic entering this chain will
just return back and continue to match against the other rules until
the end of the firewall.

For example, to allow HTTPS requests based on a knock write:

    server https accept with knock hidden
                    
then configure knockd to enable the HTTPS service with:

    iptables -A knock_hidden -s %IP% -j ACCEPT
                    
and disable it with:

    iptables -D knock_hidden -s %IP% -j ACCEPT
                    
You can use the same knock *name* in more than one FireHOL rule to
enable/disable all the services based on a single knockd
configuration entry.

> **Note**
>
> There is no need to match anything other than the IP in knockd.
> FireHOL already matches everything else needed for its rules to
> work.

##### reject
 

`reject` discards the traffic matching the rules and sends a
rejecting message back to the sender.

##### reject with *message*
 

When used with `with` the specific message to return can be
specified. Run `iptables -j REJECT --help` for a list of the
`--reject-with` values which can be used for *message*. See
[REJECT WITH MESSAGES][] for some examples.

The default (no *message* specified) is to send `tcp-reset` when
dealing with TCP connections and `icmp-port-unreachable` for all
other protocols.

For example:

~~~~

UNMATCHED_INPUT_POLICY="reject with host-prohib"

policy reject with host-unreach

server ident reject with tcp-reset
~~~~
                  

##### drop; deny
 

`drop` discards the traffic matching the rules. It does so silently
and the sender will need to timeout to conclude it cannot reach the
service.

`deny` is a synonym for `drop`. For example, either of these would
silently discard SMTP traffic:

~~~~
server smtp drop

server smtp deny
~~~~
                  

##### return
 

`return` will return the flow of processing to the parent of the
current command.

Currently, the only time `return` can be used meaningfully used is
as a policy for an interface definition. Unmatched traffic will
continue being processed with the possibility of being matched by a
later definition. For example:

    policy return
                  

##### tarpit
 

`tarpit` captures and holds incoming TCP connections open.

Connections are accepted and immediately switched to the persist
state (0 byte window), in which the remote side stops sending data
and asks to continue every 60-240 seconds.

Attempts to close the connection are ignored, forcing the remote
side to time out the connection after 12-24 minutes.

Example:

    server smtp tarpit

> **Note**
>
> As the kernel conntrack modules are always loaded by FireHOL, some
> per-connection resources will be consumed. See this [bug
> report](http://bugs.sanewall.org/sanewall/issues/10) for details.

The following actions also exist but should not be used under normal
circumstances:

##### mirror
 

`mirror` returns the traffic it receives by switching the source and
destination fields. REJECT will be used for traffic generated by the
local host.

> **Warning**
>
> The MIRROR target was removed from the Linux kernel due to its
> security implications.
>
> MIRROR is dangerous; use it with care and only if you understand
> what you are doing.

##### redirect; redirect to-port port
 

`redirect` is used internally by FireHOL helper commands.

Only FireHOL developers should need to use this action directly.


#### REJECT WITH MESSAGES
 

The following RFCs contain information relevant to these messages:

* [RFC 1812](http://www.ietf.org/rfc/rfc1812.txt)
* [RFC 1122](http://www.ietf.org/rfc/rfc1122.txt)
* [RFC 792](http://www.ietf.org/rfc/rfc0792.txt)

icmp-net-unreachable; net-unreach
:   ICMP network unreachable

    Generated by a router if a forwarding path (route) to the
    destination network is not available.

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 792.

    > **Note**
    >
    > Use with care. The sender and the routers between you and the
    > sender may conclude that the whole network your host resides in is
    > unreachable, and prevent other traffic from reaching you.

icmp-host-unreachable; host-unreach
:   ICMP host unreachable

    Generated by a router if a forwarding path (route) to the
    destination host on a directly connected network is not available
    (does not respond to ARP).

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 792.

    > **Note**
    >
    > Use with care. The sender and the routers between you and the
    > sender may conclude that your host is entirely unreachable, and
    > prevent other traffic from reaching you.

icmp-proto-unreachable; proto-unreach
:   ICMP protocol unreachable

    Generated if the transport protocol designated in a datagram is not
    supported in the transport layer of the final destination.

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 792.

icmp-port-unreachable; port-unreach
:   ICMP port unreachable

    Generated if the designated transport protocol (e.g. TCP, UDP, etc.)
    is unable to demultiplex the datagram in the transport layer of the
    final destination but has no protocol mechanism to inform the
    sender.

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 792.

    Generated by hosts to indicate that the required port is not active.

icmp-net-prohibited; net-prohib
:   ICMP communication with destination network administratively
    prohibited

    This code was intended for use by end-to-end encryption devices used
    by U.S. military agencies. Routers SHOULD use the newly defined Code
    13 (Communication Administratively Prohibited) if they
    administratively filter packets.

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 1122.

    > **Note**
    >
    > This message may not be widely understood.

icmp-host-prohibited; host-prohib
:   ICMP communication with destination host administratively prohibited

    This code was intended for use by end-to-end encryption devices used
    by U.S. military agencies. Routers SHOULD use the newly defined Code
    13 (Communication Administratively Prohibited) if they
    administratively filter packets.

    From RFC 1812, section 5.2.7.1. See RFC 1812 and RFC 1122.

    > **Note**
    >
    > This message may not be widely understood.

tcp-reset
:   TCP RST

    The port unreachable message of the TCP stack.

    See RFC 1122.

    > **Note**
    >
    > `tcp-reset` is useful when you want to prevent timeouts on
    > rejected TCP services where the client incorrectly ignores ICMP
    > port unreachable messages.


#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-params(5)][] - optional rule parameters
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

## Helper Commands for FireHOL


### firehol-iptables(5)

#### NAME
 

firehol-iptables - include custom iptables commands
<!--
extra-manpage: firehol-ip6tables.5
-->

#### SYNOPSIS
 

iptables *argument*...

ip6tables *argument*...

#### DESCRIPTION
 

The `iptables` and `ip6tables` helper commands pass all of their *argument*s
to the real iptables(8) or ip6tables(8) at the appropriate point during
run-time.

> **Note**
>
> When used in an `interface` or `router`, the result will not have a
> direct relationship to the enclosing definition as the parameters
> passed are only those you supply.

You should not use `/sbin/iptables` or `/sbin/ip6tables` directly in a
FireHOL configuration as they will run before FireHOL activates its
firewall. This means that the commands are applied to the previously
running firewall, not the new firewall, and will be lost when the new
firewall is activated.

The `iptables` and `ip6tables` helpers are provided to allow you to hook
in commands safely.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-masquerade(5)

#### NAME
 

firehol-masquerade - set up masquerading (NAT) on an interface

#### SYNOPSIS
 

masquerade *real-interface* *rule-params*

masquerade [reverse] *rule-params*

#### DESCRIPTION
 


The `masquerade` helper command sets up masquerading on the output of a
real network interface (as opposed to a FireHOL `interface` definition).

If a *real-interface* is specified the command should be used before any
`interface` or `router` definitions. Multiple values can be given separated
by whitespace, so long as they are enclosed in quotes.

If used within an `interface` definition the definition's *real-interface*
will be used.

If used within a router definition the definition's `outface`(s) will be
used, if specified. If the `reverse` option is gived, then the
definition's `inface`(s) will be used, if specified.

Unlike most commands, `masquerade` does not inherit its parent
definition's *rule-params*, it only honours its own. The `inface` and
`outface` parameters should not be used (iptables(8) does not support
inface in the POSTROUTING chain and outface will be overwritten by
FireHOL using the rules above).

> **Note**
>
> The masquerade always applies to the output of the chosen network
> interfaces.
>
> FIREHOL\_NAT will be turned on automatically (see
> [firehol-variables(5)][] ) and FireHOL will
> enable packet-forwarding in the kernel.

#### MASQUERADING AND SNAT
 

Masquerading is a special form of Source NAT (SNAT) that changes the
source of requests when they go out and replaces their original source
when they come in. This way a Linux host can become an Internet router
for a LAN of clients having unroutable IP addresses. Masquerading takes
care to re-map IP addresses and ports as required.

Masquerading is expensive compare to SNAT because it checks the IP
address of the outgoing interface every time for every packet. If your
host has a static IP address you should generally prefer SNAT.

#### EXAMPLES
 

~~~~

 # Before any interface or router
 masquerade eth0 src 192.0.2.0/24 dst not 192.0.2.0/24

 # In an interface definition to masquerade the output of its real-interface
 masquerade

 # In a router definition to masquerade the output of its outface
 masquerade

 # In a router definition to masquerade the output of its inface
 masquerade reverse
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-params(5)][] - optional rule parameters
* [firehol-nat(5)][] - nat, snat, dnat, redirect config helpers
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-tcpmss(5)

#### NAME
 

firehol-tcpmss - set the MSS of TCP SYN packets for routers

#### SYNOPSIS
 

tcpmss { *mss* | auto } [*if-list*]

#### DESCRIPTION
 

The `tcpmss` helper command sets the MSS (Maximum Segment Size) of TCP
SYN packets routed through the firewall. This can be used to overcome
situations where Path MTU Discovery is not working and packet
fragmentation is not possible.

A numeric *mss* will set MSS of TCP connections to the value given. Using
the word `auto` will set the MSS to the MTU of the outgoing interface
minus 40 (clamp-mss-to-pmtu).

If used within a `router` or `interface` definition the MSS will be applied
to outgoing traffic on the `outface`(s) of the router or interface.

If used before any router or interface definitions it will be applied to
all traffic passing through the firewall. If *if-list* is given, the MSS
will be applied only to those interfaces.

#### EXAMPLES
 

~~~~

tcpmss auto

tcpmss 500

tcpmss 500 "eth1 eth2 eth3"
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [TCPMSS target in the iptables tutorial](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html#TCPMSSTARGET)

\newpage

## Configuration Helper Commands for FireHOL


### firehol-version(5)

#### NAME
 

firehol-version - set version number of configuration file

#### SYNOPSIS
 

version 6

#### DESCRIPTION
 

The `version` helper command states the configuration file version.

If the value passed is newer than the running version of FireHOL
supports, FireHOL will not run.

You do not have to specify a version number for a configuration file,
but by doing so you will prevent FireHOL trying to process a file which
it cannot handle.

The value that FireHOL expects is increased every time that the
configuration file format changes.

> **Note**
>
> If you pass version 5 to FireHOL, it will disable IPv6 support and
> warn you that you must update your configuration.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-action(5)

#### NAME
 

firehol-action - set up custom filter actions

#### SYNOPSIS
 

action chain *name* *action*

#### DESCRIPTION
 


The `action` helper command creates an iptables(8) chain which can be used
to control the action of other firewall rules once the firewall is
running.

For example, you can setup the custom action ACT1, which by default is
ACCEPT, but can be dynamically changed to DROP, REJECT or RETURN (and
back) without restarting the firewall.

The *name* can be any chain name accepted by iptables. You should try to
keep it within 5 and 10 characters.

> **Note**
>
> The *name*s created with this command are case-sensitive.

The *action* can be any of those supported by FireHOL (see
[firehol-actions(5)][]). Only ACCEPT, REJECT, DROP,
RETURN have any meaning in this instance.

#### EXAMPLES
 

To create a custom chain and have some rules use it:

~~~~
action chain ACT1 accept

interface any world
    server smtp ACT1
    client smtp ACT1
~~~~
        
Once the firewall is running you can dynamically modify the behaviour of
the chain from the Linux command-line, as detailed below:

To insert a DROP action at the start of the chain to override the
default action (ACCEPT):

    iptables -t filter -I ACT1 -j DROP

To delete the DROP action from the start of the chain to return to the
default action:

    iptables -t filter -D ACT1 -j DROP

> **Note**
>
> If you delete all of the rules in the chain, the default will be to
> RETURN, in which case the behaviour will be as if any rules with the
> action were not present in the configuration file.

You can also create multiple chains simultaneously. To create 3 ACCEPT
and 3 DROP chains you can do the following:

~~~~
action chain "ACT1 ACT2 ACT3" accept
action chain "ACT4 ACT5 ACT6" drop
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-actions(5)][] - optional rule parameters
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-blacklist(5)

#### NAME
 

firehol-blacklist - set up a unidirectional or bidirectional blacklist

#### SYNOPSIS
 

blacklist [ full | all ] *ip*...

blacklist { input | them | him | her | it | this | these } *ip*...

#### DESCRIPTION
 


The `blacklist` helper command creates a blacklist for the *ip* list given
(which can be in quotes or not).

If the type `full` or one of its aliases is supplied, or no type is
given, a bidirectional stateless blacklist will be generated. The
firewall will REJECT all traffic going to the IP addresses and DROP all
traffic coming from them.

If the type `input` or one of its aliases is supplied, a unidirectional
stateful blacklist will be generated. Connections can be initiated to
such IP addresses, but the IP addresses will not be able to connect to
the firewall or hosts protected by it.

Any blacklists will affect all router and interface definitions. They
must be declared before the first router or interface.


#### EXAMPLES
 

~~~~
blacklist full 192.0.2.1 192.0.2.2
blacklist input "192.0.2.3 192.0.2.4"
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-classify(5)

#### NAME
 

firehol-classify - classify traffic for traffic shaping tools

#### SYNOPSIS
 

classify *class* [*rule-params*]

#### DESCRIPTION
 

The `classify` helper command puts matching traffic into the specified
traffic shaping class.

The *class* is a class as used by iptables(8) and tc(8) (e.g. MAJOR:MINOR).

The *rule-params* define a set of rule parameters to match the traffic
that is to be classified. See [firehol-params(5)][] for
more details.

Any classify commands will affect all traffic matched. They must be
declared before the first router or interface.


#### EXAMPLES
 

~~~~
 # Put all smtp traffic leaving via eth1 in class 1:1
 classify 1:1 outface eth1 proto tcp dport 25
~~~~

#### SEE ALSO
 

* [firehol-params(5)][] - optional rule parameters
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* tc(8) - show / manipulate traffic control settings
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [Linux Advanced Routing & Traffic Control HOWTO](http://lartc.org/howto/)

\newpage

### firehol-connmark(5)

#### NAME
 

firehol-connmark - set a stateful mark on a connection

#### SYNOPSIS
 

connmark { value | save | restore } *chain* *rule-params*

#### DESCRIPTION
 

The `connmark` helper command sets a mark on a whole connection. It
applies to both directions.

> **Note**
>
> To set a mark on packets matching particular rules, regardless of any
> connection, see [firehol-mark(5)][keyword-firehol-mark-helper].

The *value* is the mark value to set (a 32 bit integer). If you specify
`save` then the mark on the matched packet will be turned into a
connmark. If you specify `restore` then the matched packet will have its
mark set to the current connmark.

The *chain* will be used to find traffic to mark. It can be any of the
iptables(8) built in chains belonging to the `mangle` table. The chain
names are: INPUT, FORWARD, OUTPUT, PREROUTING and POSTROUTING. The names
are case-sensitive.

The *rule-params* define a set of rule parameters to match the traffic
that is to be marked within the chosen chain. See
[firehol-params(5)][] for more details.

Any `connmark` commands will affect all traffic matched. They must be
declared before the first router or interface.

#### EXAMPLES
 

Consider a scenario with 3 ethernet ports, where eth0 is on the local
LAN, eth1 connects to ISP 'A' and eth2 to ISP 'B'. To ensure traffic
leaves via the same ISP as it arrives from you can mark the traffic.

~~~~
 # mark connections when they arrive from the ISPs
 connmark 1 PREROUTING inface eth1
 connmark 2 PREROUTING inface eth2

 # restore the mark (from the connmark) when packets arrive from the LAN
 connmark restore OUTPUT
 connmark restore PREROUTING inface eth0
~~~~

It is then possible to use the commands from iproute2 such as ip(8), to
pick the correct routing table based on the mark on the packets.

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [firehol-mark(5)][keyword-firehol-mark-helper] - mark traffic for traffic shaping tools
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* ip(8) - show / manipulate routing, devices, policy routing and tunnels
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [Linux Advanced Routing & Traffic Control HOWTO](http://lartc.org/howto/)

\newpage

### firehol-dscp(5)

#### NAME
 

firehol-dscp - set the DSCP field in the packet header

#### SYNOPSIS
 

dscp { *value* | class *classid* } *chain* *rule-params*

#### DESCRIPTION
 

The `dscp` helper command sets the DSCP field in the header of packets
traffic, to allow QoS shaping.

> **Note**
>
> There is also a `dscp` parameter which allows matching DSCP values
> within individual rules (see [firehol-params(5)][keyword-firehol-dscp-param]).

Set *value* to a decimal or hexadecimal (0xnn) number to set an explicit
DSCP value or use `class` *classid* to use an iptables(8) DiffServ class,
such as EF, BE, CSxx or AFxx (see `iptables -j DSCP --help` for more
information).

The *chain* will be used to find traffic to mark. It can be any of the
iptables(8) built in chains belonging to the `mangle` table. The chain
names are: INPUT, FORWARD, OUTPUT, PREROUTING and POSTROUTING. The names
are case-sensitive.

The *rule-params* define a set of rule parameters to match the traffic
that is to be marked within the chosen chain. See
[firehol-params(5)][] for more details.

Any `dscp` commands will affect all traffic matched. They must be
declared before the first router or interface.

#### EXAMPLES
 

~~~~

 # set DSCP field to 32, packets sent by the local machine
 dscp 32 OUTPUT

 # set DSCP field to 32 (hex 20), packets routed by the local machine
 dscp 0x20 FORWARD

 # set DSCP to DiffServ class EF, packets routed by the local machine
 #              and destined for port TCP/25 of 198.51.100.1
 dscp class EF FORWARD proto tcp dport 25 dst 198.51.100.1
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* ip(8) - show / manipulate routing, devices, policy routing and tunnels
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [Linux Advanced Routing & Traffic Control HOWTO](http://lartc.org/howto/)

\newpage

### firehol-mac(5)

#### NAME
 

firehol-mac - ensure source IP and source MAC address match

#### SYNOPSIS
 

mac *IP* *macaddr*

#### DESCRIPTION
 


Any `mac` commands will affect all traffic destined for the firewall
host, or to be forwarded by the host. They must be declared before the
first router or interface.

> **Note**
>
> There is also a `mac` parameter which allows matching MAC addresses
> within individual rules (see [firehol-params(5)][]).

The `mac` helper command DROPs traffic from the *IP* address that was not
sent using the *macaddr* specified.

When packets are dropped, a log is produced with the label "MAC
MISSMATCH" (sic.). `mac` obeys the default log limits (see
[LOGGING][] in [firehol-params(5)][]).

> **Note**
>
> This command restricts an IP to a particular MAC address. The same MAC
> address is permitted send traffic with a different IP.


#### EXAMPLES
 

~~~~
mac 192.0.2.1    00:01:01:00:00:e6
mac 198.51.100.1 00:01:01:02:aa:e8
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-mark(5)

#### NAME
 

firehol-mark - mark traffic for traffic shaping tools

#### SYNOPSIS
 

mark *value* *chain* *rule-params*

#### DESCRIPTION
 

The `mark` helper command sets a mark on packets that can be matched by
traffic shaping tools for controlling the traffic.

> **Note**
>
> To set a mark on whole connections, see
> [firehol-connmark(5)][keyword-firehol-connmark]. There is also a `mark`
> parameter which allows matching marks within individual rules (see 
> [firehol-params(5)][keyword-firehol-mark-param]).

The *value* is the mark value to set (a 32 bit integer).

The *chain* will be used to find traffic to mark. It can be any of the
iptables(8) built in chains belonging to the `mangle` table. The chain
names are: INPUT, FORWARD, OUTPUT, PREROUTING and POSTROUTING. The names
are case-sensitive.

The *rule-params* define a set of rule parameters to match the traffic
that is to be marked within the chosen chain. See
[firehol-params(5)][] for more details.

Any `mark` commands will affect all traffic matched. They must be
declared before the first router or interface.

> **Note**
>
> If you want to do policy based routing based on iptables(8) marks, you
> will need to disable the Root Path Filtering on the interfaces
> involved (rp\_filter in sysctl).


#### EXAMPLES
 

~~~~
 # mark with 1, packets sent by the local machine
 mark 1 OUTPUT

 # mark with 2, packets routed by the local machine
 mark 2 FORWARD

 # mark with 3, packets routed by the local machine, sent from
 #              192.0.2.2 destined for port TCP/25 of 198.51.100.1
 mark 3 FORWARD proto tcp dport 25 dst 198.51.100.1 src 192.0.2.2
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [firehol-connmark(5)][keyword-firehol-connmark] - set a stateful mark on a connection
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* ip(8) - show / manipulate routing, devices, policy routing and tunnels
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [Linux Advanced Routing & Traffic Control HOWTO](http://lartc.org/howto/)

\newpage

### firehol-nat(5)

#### NAME
 

firehol-nat - set up NAT and port redirections

<!--
extra-manpage: firehol-dnat.5
extra-manpage: firehol-snat.5
extra-manpage: firehol-redirect.5
-->

#### SYNOPSIS 
 

{ nat to-destination | dnat [to] } *ipaddr*[:*port*] [*rule-params*]

{ nat to-source | snat [to] } *ipaddr*[:*port*] [*rule-params*]

{ nat redirect-to | redirect [to] } *port*[-*range*] [*rule-params*]

#### DESCRIPTION
 

Destination NAT is provided by `nat to-destination` and its synonym `dnat`.

Source NAT is provided by `nat to-source` and its synonym `snat`.

Redirection to a port on the local host is provided by `nat redirect-to`
and its synonym `redirect`.

The *port* part of the new address is optional with SNAT and DNAT; if not
specified it will not be changed.

When you apply NAT to a packet, the Linux kernel will track the changes
it makes, so that when it sees replies the transformation will be
applied in the opposite direction. For instance if you changed the
destination port of a packet from 80 to 8080, when a reply comes back,
its source is set as 80. This means the original sender is not aware
a transformation is happening.

> **Note**
>
> The *rule-params* are used only to determine the traffic that will be
> matched for NAT in these commands, not to permit traffic to flow.
>
> Applying NAT does not automatically create rules to allow the traffic to
> pass. You will still need to include client or server entries in an
> interface or router to allow the traffic.
>
> When using `dnat` or `redirect`, the transformation is in the PREROUTING
> chain of the NAT table and happens before normal rules are matched, so
> your client or server rule should match the "modified" traffic.
>
> When using `snat`, the transformation is in the POSTROUTING chain of the
> NAT table and happens after normal rules are matched, so your client or
> server rule should match the "unmodified" traffic.
>
> See the [netfilter flow diagram][netfilter flow diagram] if you would
> like to see how  network packets are processed by the kernel in detail.

The `nat` helper takes one of the following sub-commands:

to-destination *ipaddr*[:*port*]
:   Defines a Destination NAT (DNAT). Commonly thought of as
    port-forwarding (where packets destined for the firewall with a
    given port and protocol are sent to a different IP address and
    possibly port), DNAT is much more flexible in that any number of
    parameters can be matched before the destination information is
    rewritten.

    *ipaddr*[:*port*] is the destination address to be set in packets
    matching *rule-params*.

    If no rules are given, all forwarded traffic will be matched.
    `outface` should not be used in DNAT since the information is not
    available at the time the decision is made.

    *ipaddr*[:*port*] accepts any `--to-destination` values that
    iptables(8) accepts. Run `iptables -j DNAT --help` for more
    information. Multiple *ipaddr*[:*port*] may be specified by
    separating with spaces and enclosing with quotes.

to-source *ipaddr*[:*port*]
:   Defines a Source NAT (SNAT). SNAT is similar to masquerading but is
    more efficient for static IP addresses. You can use it to give a
    public IP address to a host which does not have one behind the
    firewall. See also [firehol-masquerade(5)][keyword-firehol-masquerade].

    *ipaddr*[:*port*] is the source address to be set in packets
    matching *rule-params*.

    If no rules are given, all forwarded traffic will be matched.
    `inface` should not be used in SNAT since the information is not
    available at the time the decision is made.

    *ipaddr*[:*port*] accepts any `--to-source` values that iptables(8)
    accepts. Run `iptables -j SNAT --help` for more information.
    Multiple *ipaddr*[:*port*] may be specified by separating with spaces
    and enclosing with quotes.

redirect-to *port*[-*range*]
:   Redirect matching traffic to the local machine. This is typically
    useful if you want to intercept some traffic and process it on the
    local machine.

    *port*[-*range*] is the port range (from-to) or single port that packets
    matching *rule-params*  will be redirected to.

    If no rules are given, all forwarded traffic will be matched.
    `outface` should not be used in REDIRECT since the information is
    not available at the time the decision is made.

#### EXAMPLES
 

~~~~

 # Port forwarding HTTP
 dnat to 192.0.2.2 proto tcp dport 80

 # Port forwarding HTTPS on to a different port internally
 dnat to 192.0.2.2:4443 proto tcp dport 443

 # Fix source for traffic leaving the firewall via eth0 with private address
 snat to 198.51.100.1 outface eth0 src 192.168.0.0/24

 # Transparent squid (running on the firewall) for some hosts
 redirect to 8080 inface eth0 src 198.51.100.0/24 proto tcp dport 80

 # Send to 192.0.2.1
 #  - all traffic arriving at or passing through the firewall
 nat to-destination 192.0.2.1

 # Send to 192.0.2.1
 #  - all traffic arriving at or passing through the firewall
 #  - which WAS going to 203.0.113.1
 nat to-destination 192.0.2.1 dst 203.0.113.1

 # Send to 192.0.2.1
 #  - TCP traffic arriving at or passing through the firewall
 #  - which WAS going to 203.0.113.1
 nat to-destination 192.0.2.1 proto tcp dst 203.0.113.1

 # Send to 192.0.2.1
 #  - TCP traffic arriving at or passing through the firewall
 #  - which WAS going to 203.0.113.1, port 25
 nat to-destination 192.0.2.1 proto tcp dport 25 dst 203.0.113.1
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-params(5)][] - optional rule parameters
* [firehol-masquerade(5)][keyword-firehol-masquerade] - masquerade helper
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)
* [NAT HOWTO](http://www.netfilter.org/documentation/HOWTO/NAT-HOWTO-6.html)
* [netfilter flow diagram][netfilter flow diagram]

[netfilter flow diagram]: http://upload.wikimedia.org/wikipedia/commons/3/37/Netfilter-packet-flow.svg

\newpage

### firehol-proxy(5)

#### NAME
 

firehol-proxy - set up a transparent TCP, HTTP or squid proxy

<!--
extra-manpage: firehol-transparent_proxy.5
extra-manpage: firehol-transparent_squid.5
  -->

#### SYNOPSIS
 

transparent\_proxy *service* *port* *user* *rule-params*

transparent\_squid *port* *user* *rule-params*

#### DESCRIPTION
 

The `transparent_proxy` helper command sets up transparent caching for
TCP traffic.

The `transparent_squid` helper command sets up the special case for HTTP
traffic with *service* implicitly set to 80.

> **Note**
>
> The proxy application must be running on the firewall host at port
> *port* with the credentials of the local user *user* (which may be a
> space-delimited list enclosed in quotes) serving requests appropriate
> to the TCP port service.

The *rule-params* define a set of rule parameters to define the traffic
that is to be proxied. See [firehol-params(5)][] for
more details.

For traffic destined for the firewall host or passing through the
firewall, do not use the `outface` parameter because the rules are applied
before the routing decision and so the outgoing interface will not be
known.

An empty *user* string ("") disables caching of locally-generated traffic.
Otherwise, traffic starting from the firewall is captured, except that
traffic generated by the local user(s) *user*. The `inface`, `outface` and
`src` *rule-params* are all ignored for locally-generated traffic.

#### EXAMPLES
 

~~~~

transparent_proxy 80 3128 squid inface eth0 src 192.0.2.0/24
transparent_squid 3128 squid inface eth0 src 192.0.2.0/24

transparent_proxy "80 3128 8080" 3128 "squid privoxy root bin" \
      inface not "ppp+ ipsec+" dst not "a.not.proxied.server"
transparent_squid "80 3128 8080" "squid privoxy root bin" \
      inface not "ppp+ ipsec+" dst not "non.proxied.server"
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-interface(5)][keyword-firehol-interface] - interface definition
* [firehol-router(5)][keyword-firehol-router] - router definition
* [firehol-params(5)][] - optional rule parameters
* [firehol-nat(5)][] - nat, snat, dnat, redirect config helpers
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-tos(5)

#### NAME
 

firehol-tos - set the Type of Service (TOS) of packets

#### SYNOPSIS
 

tos *value* *chain* [*rule-params*]

#### DESCRIPTION
 

The `tos` helper command sets the Type of Service (TOS) field in packet
headers.

> **Note**
>
> There is also a `tos` parameter which allows matching TOS values
> within individual rules (see [firehol-params(5)][keyword-firehol-tos-param]).

The *value* can be an integer number (decimal or hexadecimal) or one of
the descriptive values accepted by iptables(8) (run
`iptables -j TOS --help` for a list).

The *chain* will be used to find traffic to mark. It can be any of the
iptables(8) built in chains belonging to the `mangle` table. The chain
names are: INPUT, FORWARD, OUTPUT, PREROUTING and POSTROUTING. These names
are case-sensitive.

The *rule-params* define a set of rule parameters to match the traffic
that is to be marked within the chosen chain.
See [firehol-params(5)][] for more details.

Any `tos` commands will affect all traffic matched. They must be
declared before the first `router` or `interface`.

#### EXAMPLES
 

~~~~

 # set TOS to 16, packets sent by the local machine
 tos 16 OUTPUT

 # set TOS to 0x10 (16), packets routed by the local machine
 tos 0x10 FORWARD

 # set TOS to Maximize-Throughput (8), packets routed by the local
 #              machine, destined for port TCP/25 of 198.51.100.1
 tos Maximize-Throughput FORWARD proto tcp dport 25 dst 198.51.100.1
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-params(5)][] - optional rule parameters
* [firehol-tosfix(5)][keyword-firehol-tosfix] - tosfix config helper
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

### firehol-tosfix(5)

#### NAME
 

firehol-tosfix - apply suggested TOS values to packets

#### SYNOPSIS
 

tosfix

#### DESCRIPTION
 

The `tosfix` helper command sets the Type of Service (TOS) field in
packet headers based on the suggestions given by Erik Hensema in
[iptables and tc shaping
tricks](http://www.docum.org/docum.org/faq/cache/49.html).

The following TOS values are set:

-   All TCP ACK packets with length less than 128 bytes are assigned
    Minimize-Delay, while bigger ones are assigned Maximize-Throughput

-   All packets with TOS Minimize-Delay, that are bigger than 512 bytes
    are set to Maximize-Throughput, except for short bursts of 2 packets
    per second

The `tosfix` command must be used before the first router or interface.

#### EXAMPLE
 

~~~~
tosfix
~~~~

#### SEE ALSO
 

* [firehol(1)][] - FireHOL program
* [firehol.conf(5)][] - FireHOL configuration
* [firehol-tos(5)][keyword-firehol-tos-helper] - tosfix config helper
* [iptables(8)](http://ipset.netfilter.org/iptables.man.html) - administration tool for IPv4 firewalls
* [ip6tables(8)](http://ipset.netfilter.org/ip6tables.man.html) - administration tool for IPv6 firewalls
* [FireHOL Website](http://firehol.org/)
* [FireHOL Online PDF Manual](http://firehol.org/firehol-manual.pdf)
* [FireHOL Online HTML Manual](http://firehol.org/manual)

\newpage

# Services Reference for FireHOL


### firehol-services(5)

#### NAME
 

firehol-services - FireHOL services list

#### SYNOPSIS
 


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

#### DESCRIPTION
 


##### service: AH
 

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

[WIKI-AH]: http://en.wikipedia.org/wiki/IPsec#Authentication_Header

    Notes

    > For more information see this
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#AH.ipsec)
    > and [RFC 2402](http://www.ietf.org/rfc/rfc2402.txt).

##### service: all
 

Match all traffic
:   Example:

    ~~~~
      server all accept
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * all

    Client Ports:

    * all

    Notes

    > Matches all traffic (all protocols, ports, etc) while ensuring
    > that required kernel modules are loaded.
    > 
    > This service may indirectly setup a set of other services,
    > if they require kernel modules to be loaded.
    > The following complex services are activated:
    > 
    > *ftp* *irc*

##### service: amanda
 

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


##### service: any
 

Match all traffic (without modules or indirect)
:   Example:

    ~~~~
      server any *myname* accept proto 47
    ~~~~

    Service Type:

    * complex

    Server Ports:

    * all

    Client Ports:

    * all

    Notes

    > Matches all traffic (all protocols, ports, etc), but does
    > not care about kernel modules and does not activate any
    > other service indirectly. In combination with the
    > [firehol-params(5)](#firehol-params5)
    > this service can match unusual traffic (e.g. GRE - protocol 47).
    > 
    > Note that you have to supply your own name in addition to
    > "any".

##### service: anystateless
 

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

##### service: apcupsd
 

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

[HOME-apcupsd]: http://www.apcupsd.com
[WIKI-apcupsd]: http://en.wikipedia.org/wiki/Apcupsd

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

##### service: apcupsdnis
 

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

[HOME-apcupsdnis]: http://www.apcupsd.com
[WIKI-apcupsdnis]: http://en.wikipedia.org/wiki/Apcupsd

    Notes

    > This service allows the remote WEB interfaces of
    > [APCUPSD](http://www.apcupsd.com/), to connect
    > and get information from the server directly connected to
    > the UPS device.

##### service: aptproxy
 

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


##### service: asterisk
 

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

[HOME-asterisk]: http://www.asterisk.org
[WIKI-asterisk]: http://en.wikipedia.org/wiki/Asterisk_PBX

    Notes

    > This service refers only to the manager interface of asterisk.
    > You should normally enable
    > [sip][keyword-service-sip],
    > [h323][keyword-service-h323],
    > [rtp][keyword-service-rtp], etc. at the
    > firewall level, if you enable the relative channel drivers
    > of asterisk.

##### service: cups
 

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


##### service: custom
 

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

##### service: cvspserver
 

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


##### service: darkstat
 

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

[HOME-darkstat]: http://unix4lyfe.org/darkstat/


##### service: daytime
 

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


##### service: dcc
 

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

[WIKI-dcc]: http://en.wikipedia.org/wiki/Distributed_Checksum_Clearinghouse

    Notes

    > See also this
    > [DCC FAQ](http://www.rhyolite.com/dcc/FAQ.html#firewall-ports).

##### service: dcpp
 

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


##### service: dhcp
 

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

[WIKI-dhcp]: http://en.wikipedia.org/wiki/Dhcp

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

##### service: dhcprelay
 

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

[WIKI-dhcprelay]: http://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol#DHCP_relaying

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

##### service: dhcpv6
 

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

[WIKI-dhcpv6]: https://en.wikipedia.org/wiki/DHCPv6

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

##### service: dict
 

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

[WIKI-dict]: http://en.wikipedia.org/wiki/DICT

    Notes

    > See
    > [RFC2229](http://www.ietf.org/rfc/rfc2229.txt).

##### service: distcc
 

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

[HOME-distcc]: http://distcc.samba.org/
[WIKI-distcc]: http://en.wikipedia.org/wiki/Distcc

    Notes

    > For distcc security, please check the
    > [distcc security design](http://distcc.googlecode.com/svn/trunk/doc/web/security.html).

##### service: dns
 

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

[WIKI-dns]: http://en.wikipedia.org/wiki/Domain_Name_System

    Notes

    > On very busy DNS servers you may see a few dropped DNS
    > packets in your logs. This is normal. The iptables
    > connection tracker will timeout the session and lose
    > unmatched DNS packets that arrive too late to be useful.

##### service: echo
 

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


##### service: emule
 

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

[HOME-emule]: http://www.emule-project.com

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

##### service: eserver
 

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


##### service: ESP
 

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

[WIKI-ESP]: http://en.wikipedia.org/wiki/IPsec#Encapsulating_Security_Payload

    Notes

    > For more information see this
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#ESP.ipsec)
    > [RFC 2406](http://www.ietf.org/rfc/rfc2406.txt).

##### service: finger
 

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


##### service: ftp
 

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

[WIKI-ftp]: http://en.wikipedia.org/wiki/Ftp

    Notes

    > The FTP service matches both active and passive FTP
    > connections.

##### service: gift
 

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

[HOME-gift]: http://gift.sourceforge.net
[WIKI-gift]: http://en.wikipedia.org/wiki/GiFT

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

##### service: giftui
 

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

[HOME-giftui]: http://gift.sourceforge.net
[WIKI-giftui]: http://en.wikipedia.org/wiki/GiFT

    Notes

    > This service refers only to the user interface ports offered
    > by giFT. To allow gift accept P2P requests, use the
    > [gift][keyword-service-gift].

##### service: gkrellmd
 

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


##### service: GRE
 

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

[WIKI-GRE]: http://en.wikipedia.org/wiki/Generic_Routing_Encapsulation

    Notes

    > Protocol No 47.
    > 
    > For more information see RFC [RFC 2784](http://www.ietf.org/rfc/rfc2784.txt).

##### service: h323
 

H.323 VoIP
:   Example:

    ~~~~
      server h323 accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * tcp/1720

    Client Ports:

    * default

    Netfilter Modules

    * nf_conntrack_h323 [CONFIG_NF_CONNTRACK_H323](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_H323.html)

    Netfilter NAT Modules

    * nf_nat_h323 [CONFIG_NF_NAT_H323](http://cateee.net/lkddb/web-lkddb/NF_NAT_H323.html)

    Links

    * [Wikipedia][WIKI-h323]

[WIKI-h323]: http://en.wikipedia.org/wiki/H323


##### service: heartbeat
 

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

[HOME-heartbeat]: http://www.linux-ha.org/

    Notes

    > This FireHOL service has been designed such a way that it
    > will allow multiple heartbeat clusters on the same LAN.

##### service: http
 

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


##### service: httpalt
 

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

[WIKI-httpalt]: http://en.wikipedia.org/wiki/Http

    Notes

    > This port is commonly used by web servers, web proxies
    > and caches where the standard [http][keyword-service-http]
    > port is not available or can or should not be used.

##### service: https
 

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


##### service: hylafax
 

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

[HOME-hylafax]: http://www.hylafax.org
[WIKI-hylafax]: http://en.wikipedia.org/wiki/Hylafax

    Notes

    > This service allows incoming requests to server port
    > tcp/4559 and outgoing from server port tcp/4558.
    > 
    > The correct operation of this service has not been verified.
    > 
    > USE THIS WITH CARE. A HYLAFAX CLIENT MAY OPEN ALL TCP
    > UNPRIVILEGED PORTS TO ANYONE (from port tcp/4558).

##### service: iax
 

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

[HOME-iax]: http://www.asterisk.org
[WIKI-iax]: http://en.wikipedia.org/wiki/Iax

    Notes

    > This service refers to IAX version 1.
    > There is also [iax2][keyword-service-iax2].

##### service: iax2
 

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

[HOME-iax2]: http://www.asterisk.org
[WIKI-iax2]: http://en.wikipedia.org/wiki/Iax

    Notes

    > This service refers to IAX version 2.
    > There is also [iax][keyword-service-iax].

##### service: ICMP
 

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


##### service: icmp
 

Internet Control Message Protocol
:   Alias for [ICMP][keyword-service-ICMP]


##### service: ICMPV6
 

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


##### service: icmpv6
 

Internet Control Message Protocol v6
:   Alias for [ICMPV6][keyword-service-ICMPV6]


##### service: icp
 

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


##### service: ident
 

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


##### service: imap
 

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


##### service: imaps
 

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


##### service: ipsecnatt
 

NAT traversal and IPsec
:   Service Type:

    * simple

    Server Ports:

    * udp/4500

    Client Ports:

    * any

    Links

    * [Wikipedia][WIKI-ipsecnatt]

[WIKI-ipsecnatt]: http://en.wikipedia.org/wiki/NAT_traversal#NAT_traversal_and_IPsec


##### service: ipv6error
 

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

##### service: ipv6neigh
 

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

[WIKI-ipv6neigh]: https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol

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

##### service: ipv6router
 

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

[WIKI-ipv6router]: https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol

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

##### service: irc
 

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


##### service: isakmp
 

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

[WIKI-isakmp]: http://en.wikipedia.org/wiki/ISAKMP

    Notes

    > For more information see the
    > [Archive of the FreeS/WAN documentation](http://web.archive.org/web/20100918134143/http://www.freeswan.org/freeswan_trees/freeswan-1.99/doc/ipsec.html#IKE.ipsec)

##### service: jabber
 

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

[WIKI-jabber]: http://en.wikipedia.org/wiki/Jabber

    Notes

    > Allows clear and SSL client-to-server connections.

##### service: jabberd
 

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

[WIKI-jabberd]: http://en.wikipedia.org/wiki/Jabber

    Notes

    > Allows clear and SSL client-to-server and server-to-server
    > connections.
    > 
    > Use this service for a jabberd server. In all other cases,
    > use the [jabber][keyword-service-jabber].

##### service: l2tp
 

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


##### service: ldap
 

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


##### service: ldaps
 

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


##### service: lpd
 

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

[WIKI-lpd]: http://en.wikipedia.org/wiki/Line_Printer_Daemon_protocol

    Notes

    > LPD is documented in
    > [RFC 1179](http://www.ietf.org/rfc/rfc1179.txt).
    > 
    > Since many operating systems incorrectly use the non-default
    > client ports for LPD access, this definition allows any
    > client port to access the service (in addition to
    > the RFC defined 721 to 731 inclusive).

##### service: microsoft-ds
 

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

##### service: mms
 

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

[WIKI-mms]: http://en.wikipedia.org/wiki/Microsoft_Media_Server

    Notes

    > Microsoft's proprietary network streaming protocol used
    > to transfer unicast data in Windows Media Services
    > (previously called NetShow Services).

##### service: msn
 

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


##### service: msnp
 

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


##### service: ms-ds
 

Direct Hosted (NETBIOS-less) SMB
:   Alias for [microsoft_ds][keyword-service-microsoft_ds]


##### service: multicast
 

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

[WIKI-multicast]: http://en.wikipedia.org/wiki/Multicast

    Notes

    > The multicast service matches all packets sent to
    > the $MULTICAST_IPS addresses using IGMP or UDP.
    > For IPv4 that means 224.0.0.0/4 and for IPv6 FF00::/16.

##### service: mysql
 

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


##### service: netbackup
 

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

[WIKI-netbackup]: http://en.wikipedia.org/wiki/Netbackup

    Notes

    > To use this service you must define it as both client and
    > server in NetBackup clients and NetBackup servers.

##### service: netbios-dgm
 

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

[WIKI-netbios_dgm]: http://en.wikipedia.org/wiki/Netbios#Datagram_distribution_service

    Notes

    > See also the [samba][keyword-service-samba].
    > 
    > Keep in mind that this service broadcasts (to the broadcast
    > address of your LAN) UDP packets. If you place this service
    > within an interface that has a dst parameter, remember to
    > include (in the dst parameter) the broadcast address of your
    > LAN too.

##### service: netbios-ns
 

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

[WIKI-netbios_ns]: http://en.wikipedia.org/wiki/Netbios#Name_service

    Notes

    > See also the [samba][keyword-service-samba].

##### service: netbios-ssn
 

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

[WIKI-netbios_ssn]: http://en.wikipedia.org/wiki/Netbios#Session_service

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

##### service: nfs
 

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

[WIKI-nfs]: http://en.wikipedia.org/wiki/Network_File_System_%28protocol%29

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

##### service: nis
 

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

[WIKI-nis]: http://en.wikipedia.org/wiki/Network_Information_Service

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
    > [Carlos Rodrigues](http://sourceforge.net/p/firehol/feature-requests/20).
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

##### service: nntp
 

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


##### service: nntps
 

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


##### service: nrpe
 

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


##### service: ntp
 

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


##### service: nut
 

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


##### service: nxserver
 

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

[WIKI-nxserver]: http://en.wikipedia.org/wiki/NX_Server

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

##### service: openvpn
 

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


##### service: oracle
 

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


##### service: OSPF
 

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


##### service: ping
 

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

[WIKI-ping]: http://en.wikipedia.org/wiki/Ping

    Notes

    > This services matches requests of protocol ICMP and type
    > echo-request (TYPE=8) and their replies of type echo-reply
    > (TYPE=0).
    > 
    > The ping service is stateful.

##### service: pop3
 

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


##### service: pop3s
 

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


##### service: portmap
 

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


##### service: postgres
 

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


##### service: pptp
 

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


##### service: privoxy
 

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


##### service: radius
 

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


##### service: radiusold
 

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


##### service: radiusoldproxy
 

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


##### service: radiusproxy
 

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


##### service: rdp
 

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

[WIKI-rdp]: http://en.wikipedia.org/wiki/Remote_Desktop_Protocol

    Notes

    > Remote Desktop Protocol is also known also as
    > Terminal Services.

##### service: rndc
 

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


##### service: rsync
 

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


##### service: rtp
 

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

[WIKI-rtp]: http://en.wikipedia.org/wiki/Real-time_Transport_Protocol

    Notes

    > RTP ports are generally all the UDP ports.
    > This definition narrows down RTP ports to UDP 10000 to 20000.

##### service: samba
 

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

[HOME-samba]: http://www.samba.org/
[WIKI-samba]: http://en.wikipedia.org/wiki/Samba_(software)

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

##### service: sane
 

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


##### service: sip
 

Session Initiation Protocol
:   Example:

    ~~~~
      server sip accept
    ~~~~

    Service Type:

    * simple

    Server Ports:

    * udp/5060

    Client Ports:

    * 5060 default

    Netfilter Modules

    * nf_conntrack_sip [CONFIG_NF_CONNTRACK_SIP](http://cateee.net/lkddb/web-lkddb/NF_CONNTRACK_SIP.html)

    Netfilter NAT Modules

    * nf_nat_sip [CONFIG_NF_NAT_SIP](http://cateee.net/lkddb/web-lkddb/NF_NAT_SIP.html)

    Links

    * [Wikipedia][WIKI-sip]

[WIKI-sip]: http://en.wikipedia.org/wiki/Session_Initiation_Protocol

    Notes

    > [SIP](http://www.voip-info.org/wiki/view/SIP) is an IETF
    > standard protocol (RFC 2543) for initiating interactive user
    > sessions involving multimedia elements such as video, voice,
    > chat, gaming, etc. SIP works in the application layer of
    > the OSI communications model.

##### service: smtp
 

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


##### service: smtps
 

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


##### service: snmp
 

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


##### service: snmptrap
 

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

[WIKI-snmptrap]: http://en.wikipedia.org/wiki/Simple_Network_Management_Protocol#Trap

    Notes

    > An SNMP trap is a notification from an agent to a manager.

##### service: socks
 

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

[WIKI-socks]: http://en.wikipedia.org/wiki/SOCKS

    Notes

    > See also [RFC 1928](http://www.ietf.org/rfc/rfc1928.txt).

##### service: squid
 

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


##### service: ssh
 

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


##### service: stun
 

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

[WIKI-stun]: http://en.wikipedia.org/wiki/STUN

    Notes

    > [STUN](http://www.voip-info.org/wiki/view/STUN)
    > is a protocol for assisting devices behind a NAT firewall or
    > router with their packet routing.

##### service: submission
 

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

[WIKI-submission]: http://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol

    Notes

    > Submission is essentially normal SMTP with an SSL/TLS
    > negotiation.

##### service: sunrpc
 

Open Network Computing Remote Procedure Call - Port Mapper
:   Alias for [portmap][keyword-service-portmap]


##### service: swat
 

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


##### service: syslog
 

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

    * syslog default

    Links

    * [Wikipedia][WIKI-syslog]

[WIKI-syslog]: http://en.wikipedia.org/wiki/Syslog


##### service: telnet
 

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


##### service: tftp
 

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


##### service: time
 

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


##### service: timestamp
 

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

[WIKI-timestamp]: http://en.wikipedia.org/wiki/Internet_Control_Message_Protocol#Timestamp

    Notes

    > This services matches requests of protocol ICMP and type
    > timestamp-request (TYPE=13) and their replies of type
    > timestamp-reply (TYPE=14).
    > 
    > The timestamp service is stateful.

##### service: tomcat
 

HTTP alternate port
:   Alias for [httpalt][keyword-service-httpalt]


##### service: upnp
 

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

[HOME-upnp]: http://upnp.sourceforge.net/
[WIKI-upnp]: http://en.wikipedia.org/wiki/Universal_Plug_and_Play

    Notes

    > For a Linux implementation see:
    > [Linux IGD](http://linux-igd.sourceforge.net/).

##### service: uucp
 

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


##### service: vmware
 

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

##### service: vmwareauth
 

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

##### service: vmwareweb
 

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

##### service: vnc
 

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

[WIKI-vnc]: http://en.wikipedia.org/wiki/Virtual_Network_Computing

    Notes

    > VNC is a graphical desktop sharing protocol.

##### service: webcache
 

HTTP alternate port
:   Alias for [httpalt][keyword-service-httpalt]


##### service: webmin
 

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


##### service: whois
 

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


##### service: xbox
 

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

##### service: xdmcp
 

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

[WIKI-xdmcp]: http://en.wikipedia.org/wiki/X_display_manager_(program_type)#X_Display_Manager_Control_Protocol

    Notes

    > See [Gnome Display Manager](http://www.jirka.org/gdm-documentation/x70.html)
    > for a discussion about XDMCP and firewalls (Gnome Display
    > Manager is a replacement for XDM).

\newpage

<!--
  This file lists keywords associated with FireHOL services
  and associates them with anchors in the HTML.
  -->

[keyword-service-AH]: #service-ah
[keyword-service-all]: #service-all
[keyword-service-amanda]: #service-amanda
[keyword-service-any]: #service-any
[keyword-service-anystateless]: #service-anystateless
[keyword-service-apcupsd]: #service-apcupsd
[keyword-service-apcupsdnis]: #service-apcupsdnis
[keyword-service-aptproxy]: #service-aptproxy
[keyword-service-asterisk]: #service-asterisk
[keyword-service-cups]: #service-cups
[keyword-service-custom]: #service-custom
[keyword-service-cvspserver]: #service-cvspserver
[keyword-service-darkstat]: #service-darkstat
[keyword-service-daytime]: #service-daytime
[keyword-service-dcc]: #service-dcc
[keyword-service-dcpp]: #service-dcpp
[keyword-service-dhcp]: #service-dhcp
[keyword-service-dhcprelay]: #service-dhcprelay
[keyword-service-dhcpv6]: #service-dhcpv6
[keyword-service-dict]: #service-dict
[keyword-service-distcc]: #service-distcc
[keyword-service-dns]: #service-dns
[keyword-service-echo]: #service-echo
[keyword-service-emule]: #service-emule
[keyword-service-eserver]: #service-eserver
[keyword-service-ESP]: #service-esp
[keyword-service-finger]: #service-finger
[keyword-service-ftp]: #service-ftp
[keyword-service-gift]: #service-gift
[keyword-service-giftui]: #service-giftui
[keyword-service-gkrellmd]: #service-gkrellmd
[keyword-service-GRE]: #service-gre
[keyword-service-h323]: #service-h323
[keyword-service-heartbeat]: #service-heartbeat
[keyword-service-http]: #service-http
[keyword-service-httpalt]: #service-httpalt
[keyword-service-https]: #service-https
[keyword-service-hylafax]: #service-hylafax
[keyword-service-iax]: #service-iax
[keyword-service-iax2]: #service-iax2
[keyword-service-ICMP]: #service-icmp
[keyword-service-icmp]: #service-icmp
[keyword-service-ICMPV6]: #service-icmpv6
[keyword-service-icmpv6]: #service-icmpv6
[keyword-service-icp]: #service-icp
[keyword-service-ident]: #service-ident
[keyword-service-imap]: #service-imap
[keyword-service-imaps]: #service-imaps
[keyword-service-ipsecnatt]: #service-ipsecnatt
[keyword-service-ipv6error]: #service-ipv6error
[keyword-service-ipv6neigh]: #service-ipv6neigh
[keyword-service-ipv6router]: #service-ipv6router
[keyword-service-irc]: #service-irc
[keyword-service-isakmp]: #service-isakmp
[keyword-service-jabber]: #service-jabber
[keyword-service-jabberd]: #service-jabberd
[keyword-service-l2tp]: #service-l2tp
[keyword-service-ldap]: #service-ldap
[keyword-service-ldaps]: #service-ldaps
[keyword-service-lpd]: #service-lpd
[keyword-service-microsoft_ds]: #service-microsoft-ds
[keyword-service-mms]: #service-mms
[keyword-service-msn]: #service-msn
[keyword-service-msnp]: #service-msnp
[keyword-service-ms_ds]: #service-ms-ds
[keyword-service-multicast]: #service-multicast
[keyword-service-mysql]: #service-mysql
[keyword-service-netbackup]: #service-netbackup
[keyword-service-netbios_dgm]: #service-netbios-dgm
[keyword-service-netbios_ns]: #service-netbios-ns
[keyword-service-netbios_ssn]: #service-netbios-ssn
[keyword-service-nfs]: #service-nfs
[keyword-service-nis]: #service-nis
[keyword-service-nntp]: #service-nntp
[keyword-service-nntps]: #service-nntps
[keyword-service-nrpe]: #service-nrpe
[keyword-service-ntp]: #service-ntp
[keyword-service-nut]: #service-nut
[keyword-service-nxserver]: #service-nxserver
[keyword-service-openvpn]: #service-openvpn
[keyword-service-oracle]: #service-oracle
[keyword-service-OSPF]: #service-ospf
[keyword-service-ping]: #service-ping
[keyword-service-pop3]: #service-pop3
[keyword-service-pop3s]: #service-pop3s
[keyword-service-portmap]: #service-portmap
[keyword-service-postgres]: #service-postgres
[keyword-service-pptp]: #service-pptp
[keyword-service-privoxy]: #service-privoxy
[keyword-service-radius]: #service-radius
[keyword-service-radiusold]: #service-radiusold
[keyword-service-radiusoldproxy]: #service-radiusoldproxy
[keyword-service-radiusproxy]: #service-radiusproxy
[keyword-service-rdp]: #service-rdp
[keyword-service-rndc]: #service-rndc
[keyword-service-rsync]: #service-rsync
[keyword-service-rtp]: #service-rtp
[keyword-service-samba]: #service-samba
[keyword-service-sane]: #service-sane
[keyword-service-sip]: #service-sip
[keyword-service-smtp]: #service-smtp
[keyword-service-smtps]: #service-smtps
[keyword-service-snmp]: #service-snmp
[keyword-service-snmptrap]: #service-snmptrap
[keyword-service-socks]: #service-socks
[keyword-service-squid]: #service-squid
[keyword-service-ssh]: #service-ssh
[keyword-service-stun]: #service-stun
[keyword-service-submission]: #service-submission
[keyword-service-sunrpc]: #service-sunrpc
[keyword-service-swat]: #service-swat
[keyword-service-syslog]: #service-syslog
[keyword-service-telnet]: #service-telnet
[keyword-service-tftp]: #service-tftp
[keyword-service-time]: #service-time
[keyword-service-timestamp]: #service-timestamp
[keyword-service-tomcat]: #service-tomcat
[keyword-service-upnp]: #service-upnp
[keyword-service-uucp]: #service-uucp
[keyword-service-vmware]: #service-vmware
[keyword-service-vmwareauth]: #service-vmwareauth
[keyword-service-vmwareweb]: #service-vmwareweb
[keyword-service-vnc]: #service-vnc
[keyword-service-webcache]: #service-webcache
[keyword-service-webmin]: #service-webmin
[keyword-service-whois]: #service-whois
[keyword-service-xbox]: #service-xbox
[keyword-service-xdmcp]: #service-xdmcp


<!--
    This file lists internal references within the manual
    and associates them with anchors in the output.

    Note that the blank line after this comment is required, to keep
    pandoc(1) happy when formatting.
  -->

[ADDING SERVICES]: #adding-services
[CONFIGURATION HELPER COMMANDS]: #configuration-helper-commands
[HELPER COMMANDS]: #helper-commands
[INTERFACE SUBCOMMANDS]: #interface-subcommands
[REJECT WITH MESSAGES]: #reject-with-messages
[ROUTER SUBCOMMANDS]: #router-subcommands

[firehol(1)]: #firehol1
[firehol-actions(5)]: #firehol-actions5
[firehol.conf(5)]: #firehol.conf5
[firehol-modifiers(5)]: #firehol-modifiers5
[firehol-nat(5)]: #firehol-nat5
[firehol-params(5)]: #firehol-params5
[firehol-proxy(5)]: #firehol-proxy5
[firehol-services(5)]: #firehol-services5
[firehol-variables(5)]: #firehol-variables5

<!--
   These links are maintained for the benefit of the website and must
   not be removed.
  -->

[keyword-manref-firehol-config-file]: #firehol.conf5
[keyword-manref-firehol-variables-control]: #firehol-variables5
[keyword-manref-firehol-variables-unroutable]: #variables-available
[keyword-manref-firehol-variables-activation]: #variables-available
[keyword-manref-firehol-helpme]: #helpme-wizard
[keyword-manref-firehol-explain]: #explain
[keyword-manref-firehol-debug]: #debug

<!--
    This file lists keywords associated with the FireHOL configuration
    language and associates them with anchors in the output.

    Note that the blank line after this comment is required, to keep
    pandoc(1) happy when formatting.
  -->

[keyword-firehol-accept]: #accept
[keyword-firehol-action]: #firehol-action5
[keyword-firehol-blacklist]: #firehol-blacklist5
[keyword-firehol-classify]: #firehol-classify5
[keyword-firehol-client]: #firehol-client5
[keyword-firehol-client4]: #firehol-client5
[keyword-firehol-client46]: #firehol-client5
[keyword-firehol-client6]: #firehol-client5
[keyword-firehol-connmark]: #firehol-connmark5
[keyword-firehol-custom]: #custom
[keyword-firehol-deny]: #drop-deny
[keyword-firehol-dnat]: #firehol-nat5
[keyword-firehol-dport]: #dport-sport
[keyword-firehol-drop]: #drop-deny
[keyword-firehol-dscp-helper]: #firehol-dscp5
[keyword-firehol-dscp-param]: #dscp
[keyword-firehol-dst]: #src-dst
[keyword-firehol-dst4]: #src-dst
[keyword-firehol-dst6]: #src-dst
[keyword-firehol-dsttype]: #srctype-dsttype
[keyword-firehol-gid]: #gid
[keyword-firehol-group]: #firehol-group5
[keyword-firehol-inface]: #inface-outface
[keyword-firehol-interface]: #firehol-interface5
[keyword-firehol-interface4]: #firehol-interface5
[keyword-firehol-interface46]: #firehol-interface5
[keyword-firehol-interface6]: #firehol-interface5
[keyword-firehol-iptables]: #firehol-iptables5
[keyword-firehol-ip6tables]: #firehol-iptables5
[keyword-firehol-ipv4]: #firehol-modifiers5
[keyword-firehol-ipv6]: #firehol-modifiers5
[keyword-firehol-log]: #log-loglimit
[keyword-firehol-loglimit]: #log-loglimit
[keyword-firehol-mac-helper]: #firehol-mac5
[keyword-firehol-mac-param]: #mac
[keyword-firehol-mark-helper]: #firehol-mark5
[keyword-firehol-mark-param]: #mark
[keyword-firehol-masquerade]: #firehol-masquerade5
[keyword-firehol-nat]: #firehol-nat5
[keyword-firehol-outface]: #inface-outface
[keyword-firehol-physin]: #physin-physout
[keyword-firehol-physout]: #physin-physout
[keyword-firehol-policy]: #firehol-policy5
[keyword-firehol-protection]: #firehol-protection5
[keyword-firehol-proto]: #proto
[keyword-firehol-redirect-helper]: #firehol-nat5
[keyword-firehol-redirect-action]: #redirect-redirect-to-port-port
[keyword-firehol-reject]: #reject
[keyword-firehol-return]: #return
[keyword-firehol-router]: #firehol-router5
[keyword-firehol-router4]: #firehol-router5
[keyword-firehol-router46]: #firehol-router5
[keyword-firehol-router6]: #firehol-router5
[keyword-firehol-route]: #firehol-server5
[keyword-firehol-route4]: #firehol-server5
[keyword-firehol-route46]: #firehol-server5
[keyword-firehol-route6]: #firehol-server5
[keyword-firehol-server]: #firehol-server5
[keyword-firehol-server4]: #firehol-server5
[keyword-firehol-server46]: #firehol-server5
[keyword-firehol-server6]: #firehol-server5
[keyword-firehol-snat]: #firehol-nat5
[keyword-firehol-sport]: #dport-sport
[keyword-firehol-src]: #src-dst
[keyword-firehol-src4]: #src-dst
[keyword-firehol-src6]: #src-dst
[keyword-firehol-srctype]: #srctype-dsttype
[keyword-firehol-tarpit]: #tarpit
[keyword-firehol-tcpmss]: #firehol-tcpmss5
[keyword-firehol-tosfix]: #firehol-tosfix5
[keyword-firehol-tos-helper]: #firehol-tos5
[keyword-firehol-tos-param]: #tos
[keyword-firehol-transparent_proxy]: #firehol-proxy5
[keyword-firehol-transparent_squid]: #firehol-proxy5
[keyword-firehol-uid]: #uid
[keyword-firehol-version]: #firehol-version5

<!--
    This file lists references used by the website and need to be maintained
    within the document.

    Note that the blank line after this comment is required, to keep
    pandoc(1) happy when formatting.
  -->

[keyword-manref-firehol-config-file]: #firehol.conf5
[keyword-manref-firehol-variables-control]: #firehol-variables5
[keyword-manref-firehol-variables-unroutable]: #variables-available
[keyword-manref-firehol-variables-activation]: #variables-available
[keyword-manref-firehol-helpme]: #helpme-wizard
[keyword-manref-firehol-explain]: #explain
[keyword-manref-firehol-debug]: #debug
