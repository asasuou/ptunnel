PingTunnel Read Me
==================

PingTunnel on Android
---------------------
This a simple porting of PingTunnel to Android system.

What is ptunnel?
----------------
Ptunnel is an application that allows you to reliably tunnel TCP connections
to a remote host using ICMP echo request and reply packets, commonly known as
ping requests and replies.


Contact details
---------------
You can the author, Daniel Stoedle, here:
   <daniels@cs.uit.no>
The official ptunnel website is located here:
   <http://www.cs.uit.no/~daniels/PingTunnel/>
The Windows port was created by Mike Miller:
   <mike@mikeage.net>


Compiling
---------
To compile ptunnel, simply run make. If everything goes well, you should end up
with a binary called ptunnel. This serves as both the client and proxy. You can
optionally install it using "make install". On Windows, run "make ptunnel.exe"
to compile the Windows binary. You will need mingw installed, as well as the
WinPcap library. WinPcap is available here:
  <http://www.winpcap.org/install/bin/WpdPack_4_0_2.zip>


Running
-------
Ptunnel works best when running as root, and usually requires running as root.
Again, from the website:

Client: ./ptunnel -p <proxy address> -lp <listen port> -da <destination address>
                  -dp <dest port> [-c <network device>] [-v <verbosity>] [-u]
                  [-x password]
Proxy: ./ptunnel [-c <network device>] [-v <verbosity>] [-u] [-x password]

The -p switch sets the address of the host on which the proxy is running. A
quick test to see if the proxy will work is simply to try pinging this host -
if you get replies, you should be able to make the tunnel work.

The -lp, -da and -dp switches set the local listening port, destination address
and destination port. For instance, to tunnel ssh connections from the client
machine via a proxy running on proxy.pingtunnel.com to the computer
login.domain.com, the following command line would be used:

sudo ./ptunnel -p proxy.pingtunnel.com -lp 8000 -da login.domain.com -dp 22

An ssh connection to login.domain.com can now be established as follows:

ssh -p 8000 localhost

If ssh complains about potential man-in-the-middle attacks, simply remove the
offending key from the known_hosts file. The warning/error is expected if you
have previously ssh'd to your local computer (i.e., ssh localhost), or you have
used ptunnel to forward ssh connections to different hosts.

Of course, for all of this to work, you need to start the proxy on your
proxy-computer (we'll call it proxy.pingtunnel.com here). Doing this is very
simple:

sudo ./ptunnel

If you find that the proxy isn't working, you will need to enable packet
capturing on the main network device. Currently this device is assumed to be
an ethernet-device (i.e., ethernet or wireless). Packet capturing is enabled by
giving the -c switch, and supplying the device name to capture packets on (for
instance eth0 or en1). The same goes for the client. On versions of Mac OS X
prior to 10.4 (Tiger), packet capturing must always be enabled (both for proxy
and client), as resent packets won't be received otherwise.

To protect yourself from others using your proxy, you can protect access to it
with a password using the <tt>-x</tt> switch. The password is never sent in
the clear, but keep in mind that it may be visible from tools like top or ps,
which can display the command line used to start an application.

Finally, the -u switch will attempt to run the proxy in unprivileged mode (i.e.,
no need for root access), and the -v switch controls the amount of output from
ptunnel. -1 indicates no output, 0 shows errors only, 1 shows info messages, 2
gives more output, 3 provides even more output, level 4 displays debug info and
level 5 displays absolutely everything, including the nasty details of sends and
receives. The -f switch allows output to be saved to a logfile.

Security features: Please see the ptunnel man-page for instructions.


Supported operating systems
---------------------------
Ptunnel supports most operating systems with libpcap, the usual POSIX functions
and a BSD sockets compatible API. In particular, it has been tested on Linux
Fedora Core 2 and Mac OS X 10.3.6 and above. As of version 0.7, ptunnel can also
be compiled on Windows, courtesy of Mike Miller, assuming mingw and WinPcap is
installed.


Credits and contributors
------------------------
Thanks to L. Peter Deutsch for his open-source MD5 implementation, included with
ptunnel, but also available here:
http://sourceforge.net/projects/libmd5-rfc/

Many thanks also to Mike Miller <mike@mikeage.net> for his work on creating a
Windows port of ptunnel.

Thanks to Sebastien Raveau <sebastien.raveau@epita.fr> for implementing various
security features and SELinux support.

Also thanks to Joe McKenzie, Steffen Wendzel and StalkR for contributing patches to
ptunnel.

License
-------
Ping Tunnel is Copyright (c) 2004-2011, Daniel Stoedle <daniels@cs.uit.no>,
Yellow Lemon Software. All rights reserved. Ping Tunnel is licensed under the
BSD License. Please see the LICENSE file for details.
