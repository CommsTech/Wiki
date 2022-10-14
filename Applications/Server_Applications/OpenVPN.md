---
title: OpenVPN
description: OpenVPN
published: true
date: 2022-07-25T13:37:12.115Z
tags: VPN, Software
editor: markdown
dateCreated: 2022-09-09T04:44:01.149Z
---
# Optimizing OpenVPN Throughput

[Hamy](https://hamy.io/author/hamy/)

Last updated on Jan 18, 2019  9 min read  [11 Comments](https://hamy.io/post/0003/optimizing-openvpn-throughput/#disqus_thread)  [Blog](https://hamy.io/categories/blog/)

In the [previous post](https://hamy.io/post/0002/openvpn-tcp-or-udp-tunneling/), I talked about OpenVPN TCP and UDP tunnels and why you should not be using TCP. In this post, I’m going to talk about optimizing the said tunnels to get the most out of them.

Believe it or not, the default OpenVPN configuration is likely not optimized for your link. It probably works but its throughput could possibly be improved if you take the time to optimize it.

A tunnel has 2 ends! Optimizing one end, does not necessarily optimizes the other. For the proper optimization of the link, both ends of the tunnel should be in your control. That means when you are using OpenVPN in server mode serving different clients that you do not have control over, the best you could do is to optimize your own end of the tunnel and use appropriate default settings suitable for the most clients.

Below are some techniques that could be used to optimize your OpenVPN tunnels.

## Compression

In today’s world where most connections are either encrypted or pre-compressed (and more commonly both), you probably should think twice before setting up compression on top of your vpn tunnel.

While it still could be an effective way to reduce the size of un-encrypted compressible packets (e.g., un-encrypted SQL connections / FTP connections transferring text files), these usages must be frequent enough to justify the use of compression.

[Security complications](https://www.mail-archive.com/openvpn-devel@lists.sourceforge.net/msg16919.html) could arise when encryption and compression are used together.

OpenVPN compresses each packets individually. Furthermore, if you load compression module on one side, it must be loaded on the other side as well.

Loading the compression module is not the same as enabling it. Enabling the module could be done after the tunnels initialization and can even be _pushed_ to the clients. [OpenVPN manual](https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage) provides more info on the subject.

If you decide to not use compression at all, prevent loading it completely by not including the `compress` (_comp-lzo_ for older versions) line. Depending on the used algorithm, mere loading the module could reduce your effective `tun-mtu` size by 1.

Traditionally, OpenVPN used `lzo` as compression algorithm but recently (OpenVPN v2.4 and above), `lz4-v2` (which itself is the successor of `lz4`), has replaced it.

Below, I will briefly cover each compression methods:

### lzo

While still supported in OpenPN v2.4 (as a deprecated feature), _comp-lzo_ option will be removed in a future release. Backward compatibility with older OpenVPN clients is provided by the use of _compress_ option. More info is available [HERE](https://community.openvpn.net/openvpn/wiki/DeprecatedOptions#a--comp-lzo).

`lzo` provides a slightly better compression ratio than the `lz4` compression (available in OpenVPN v2.4 and above). It is however, considerably slower and uses more CPU. So you probably shouldn’t be using it unless for backward compatibility reasons.

Worst-case scenario, using `lzo` might add an extra 1 byte of overhead for incompressible packets.

It is generally best to let the OpenVPN decide whether the `lzo` compression is worth enabling or not. We do this by setting the `compress` (or _comp-lzo_ in older versions) option without any arguments.

This will cause OpenVPN to periodically check the effectiveness of `lzo` compression and disable it if it does more harm than good (i.e., most packets on the transferring side end up with an extra 1 byte of overhead).

You can check the effectiveness of the `lzo` compression yourself by sending `SIGUSR2` signal to the OpenVPN process (if its running as a daemon, the output would go to the syslog file). With such statistics you could decide whether using compression is useful for your link or not.

It is also worth noting that `lzo` algorithm is in such a way that compressing a block would require relatively more resource that decompressing it.

### lz4

Possibly faster compression speed, extremely faster de-compression speed as well as optimized CPU usage, is what offered by `lz4`. Slight decrease of compression ratio is negligible comparing to its benefits.

### lz4-v2

`lz4-v2` is an optimized version of `lz4` designed specifically for OpenVPN.

#### lz4 vs lz4-v2

The main difference between `lz4` and `lz4-v2`, is a slight change of algorithm resulting in **no overhead at all** in case of an incompressible packet.

This means that the so called _adaptive_ compression (that was needed for `lzo` and even `lz4`) is required no more in `lz4-v2` and worst-case scenario, some CPU usage will be wasted on an incompressible packet without adding any overhead to the packet size (which means packet alignments would be preserved). Kudos to OpenVPN team for this.[1](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fn:1)

Just like `lzo`, it should be clear that there isn’t much use to `lz4` in place of `lz4-v2` except for compatibility with older clients.

## Cipher algorithm and size

Different ciphers have different speeds in different hardwares (ie an [AES-NI](https://en.wikipedia.org/wiki/AES_instruction_set) capable CPU). This is a hard topic to cover as it is up to you to decide whether you’d want to sacrifice better encryption to a faster tunnel or using smaller keysize to reduce the CPU load. There are countless of articles about OpenSSL ciphers, their speed and their strength. Do a google search and get yourslef familiarized with the subject. As a side note, to compare the ciphers speed in your platform take a look at `openssl speed -h` command.

## sndbuf and rcvbuf

There have been reports of speed improvement in some circumstances when these values are set to `0` [2](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fn:2).

## fast-io

This little flag which is supported in non-windows systems, improves your CPU usage when dealing with UDP packets by using non-blocking write operations. It only applies to UDP tunnels.

## float

The `--float` option adds additional 3 bytes of overhead to the clients packets. These 3 bytes contain the so called _peer-id_ which is pushed to them by the server at the time of connection. So if you don’t need the float functionality, don’t use it.

The problem however is that while according to the OpenVPN devs, “float has no effect on multipoint-servers, and never had”, the server still pushes the _peer-id_ to the clients in tls mode (even when `--float` is not specified).

You can check if this is the case for you by adding the `--verb 4` option to the server and then connect to it:

```nohighlight
PUSH: Received control message: 'PUSH_REQUEST'
SENT CONTROL [client]: 'PUSH_REPLY,ifconfig 10.1.0.2 255.255.255.0,peer-id 0,cipher AES-256-GCM' (status=1)
```

OpenVPN devs had this to say about the behavior:

> we have way too much conditional code, so we consciously decided “this feature is always-on”  
> it’s good for performance as well, as the extra 3 bytes make the rest of the packet properly 32bit-aligned = better crypto performance  
> (effectively: less CPU load, longer battery life)

It is up to you to decide on this but it is possible to remove the added 3 bytes of overhead by making the client to ignore the _peer-id_ push at the connection time:

```plaintext
--pull-filter ignore "peer-id"
```

## MTU adjustments

OpenVPN UDP packets should not be fragmented. So you need to ensure you’re not sending a packet larger than your link’s MTU. In some instances, you may need to manually [find the MTU of you link](https://hamy.io/post/000c/how-to-find-the-correct-mtu-and-mru-of-your-link/) first.

TCP tunnels usually don’t require such adjustments.

### link-mtu

The maximum size of the final UDP packet after encapsulation minus the headers. So for example if your link MTU is `1500`, the correct value for `link-mtu` would be `1472` _(1500 - 20(IP header) - 8(UDP header))_. In the OpenVPN manual it is said that it’s best to not set this value directly. However in my experience, this is in fact the best way to adjust your tun/tap link MTU properly and the `tun-mtu` value (which we will discuss later) will be derived from that. The default value of `link-mtu` however is derived from `tun-mtu` and is bigger than 1500.

### tun-mtu

The actual MTU of OpenVPN tun/tap device. This defaults to `1500`.

You can only specify either `link-mtu` or `tun-mtu` and not both. The other one will be calculated internally by OpenVPN. One other thing to note is that `link-mtu` applies to final packets (after encryption and encapsulation) while `tun-mtu` applies to the unencrypted packets which are about to enter the tun/tap device.

The `tun-mtu` of `1500` is ideal if your physical link MTU could handle it. It would provide maximum compatibility between routers along the way. However this is not always the case. Although OpenVPN is supposed to be able to discover this and act accordingly but the whole thing would collapse if you have a broken [PMTUD](https://en.wikipedia.org/wiki/Path_MTU_Discovery).  
In such case, manual intervention for adjusting MTU is required. In another post I will talk about ways to find the correct MTU of a path but assuming you already know the correct value, you subtract `28 bytes` from it and set it as `link-mtu` value and let OpenVPN calculate the right `tun-mtu` for you. Again, remember, the calculated `tun-mtu` value applies to packets before compression/encapsulation and its size highly depends on other factors like cipher algorithm, keysize, compression module, etc.

In very fast links, setting `tun-mtu` to a high value could potentially help [3](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fn:3).

### fragment

This option should generally be avoided when possible. It adds `4 bytes` of overhead to each packet. But it is there as the last resort when no other option works. With this option, OpenVPN internally fragments packets to chunks not bigger than the set value and send them over the link. The other end receives and reassembles them to create the original sent packet. This is the only instance I know that a single packet could result in more than one OpenVPN UDP packets being sent over the link.

You might also be interetsed in my [Understanding Network IP Fragmentation](https://hamy.io/post/000b/understanding-network-ip-fragmentation/) post.

### mssfix

This option only applies to TCP connections inside the tunnel. _Maximum Segment Size_ is yet another feature of TCP. This option is negotiated between peers during TCP handshaking via SYN packets. It is the maximum size of the payload each TCP packet can carry. It does not take IP and TCP header sizes into account. This option can be used in a link with broken PMTUD to at least make TCP connections possible.  
Even though MSS itself is a TCP feature, this OpenVPN option targets encapsulated UDP packets. Meaning it changes the MSS value of the TCP protocols inside the tunnel in a way that after UDP encryption/encapsulation the resulting UDP packet size (minues IP/UDP headers), would not exceed the `mssfix` value.  
So in an optimized link, `mssfix` is either disabled (set to 0) or it’s value would be the same as `link-mtu`’s.  
As a side note, `mssfix` applies to both sending AND receiving SYN packets so it is not an ideal solution for asymmetric links… but that’s for another post.

# Note on TCP tunnels

Aside from the usual [TCP tuning](https://en.wikipedia.org/wiki/TCP_tuning) and the `socket-flags TCP_NODELAY` option, probably the best optimization is to get rid of TCP tunneling as a whole and use [UDP instead](https://hamy.io/post/0002/openvpn-tcp-or-udp-tunneling/).

---

1.  [https://sourceforge.net/p/openvpn/mailman/message/34732574/](https://sourceforge.net/p/openvpn/mailman/message/34732574/) [↩︎](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fnref:1)
    
2.  [https://www.lowendtalk.com/discussion/40099/why-openvpn-is-so-slow-cool-story](https://www.lowendtalk.com/discussion/40099/why-openvpn-is-so-slow-cool-story) [↩︎](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fnref:2)
    
3.  [https://community.openvpn.net/openvpn/wiki/Gigabit_Networks_Linux](https://community.openvpn.net/openvpn/wiki/Gigabit_Networks_Linux) [↩︎](https://hamy.io/post/0003/optimizing-openvpn-throughput/#fnref:3)