---
title: IDRAC 6
description: Idrac Version 6 settings to get access to kvm
published: true
date: 2022-07-18T02:41:39.777Z
tags: networking, proxy, Server, IPMI
editor: markdown
dateCreated: 2022-07-18T02:41:39.777Z
---
# IDRAC 6

if you get the following error message. watch [this](https://www.youtube.com/watch?v=drhSo9Xl9M0) video for the solution
![[Idrac_security_application_error_idrac6.png]]

Here is an example of a modified java file for idrac access on windows 10
File is located @ C:\Program Files\Java\jre1.8.0_331\lib\security
java.security
```
# tryLast
#    KDCs in the blacklist are always tried after those not on the list.
#
# tryLess[:max_retries,timeout]
#    KDCs in the blacklist are still tried by their order in the configuration,
#    but with smaller max_retries and timeout values. max_retries and timeout
#    are optional numerical parameters (default 1 and 5000, which means once
#    and 5 seconds). Please notes that if any of the values defined here is
#    more than what is defined in krb5.conf, it will be ignored.
#
# Whenever a KDC is detected as available, it is removed from the blacklist.
# The blacklist is reset when krb5.conf is reloaded. You can add
# refreshKrb5Config=true to a JAAS configuration file so that krb5.conf is
# reloaded whenever a JAAS authentication is attempted.
#
# Example,
#   krb5.kdc.bad.policy = tryLast
#   krb5.kdc.bad.policy = tryLess:2,2000
krb5.kdc.bad.policy = tryLast

#
# This property contains a list of disabled EC Named Curves that can be included
# in the jdk.[tls|certpath|jar].disabledAlgorithms properties.  To include this
# list in any of the disabledAlgorithms properties, add the property name as
# an entry.
jdk.disabled.namedCurves = secp112r1, secp112r2, secp128r1, secp128r2, \
    secp160k1, secp160r1, secp160r2, secp192k1, secp192r1, secp224k1, \
    secp224r1, secp256k1, sect113r1, sect113r2, sect131r1, sect131r2, \
    sect163k1, sect163r1, sect163r2, sect193r1, sect193r2, sect233k1, \
    sect233r1, sect239k1, sect283k1, sect283r1, sect409k1, sect409r1, \
    sect571k1, sect571r1, X9.62 c2tnb191v1, X9.62 c2tnb191v2, \
    X9.62 c2tnb191v3, X9.62 c2tnb239v1, X9.62 c2tnb239v2, X9.62 c2tnb239v3, \
    X9.62 c2tnb359v1, X9.62 c2tnb431r1, X9.62 prime192v2, X9.62 prime192v3, \
    X9.62 prime239v1, X9.62 prime239v2, X9.62 prime239v3, brainpoolP256r1, \
    brainpoolP320r1, brainpoolP384r1, brainpoolP512r1

#
# Kerberos cross-realm referrals (RFC 6806)
#

# All DisabledAlgorithms expressions are processed in the order defined in the
# property.  This requires lower keysize constraints to be specified
# before larger keysize constraints of the same algorithm.  For example:
# "RSA keySize < 1024 & jdkCA, RSA keySize < 2048".
#
# Note: The algorithm restrictions do not apply to trust anchors or
# self-signed certificates.
#
# Note: This property is currently used by Oracle's PKIX implementation. It
# is not guaranteed to be examined and used by other implementations.
#
# Example:
#   jdk.certpath.disabledAlgorithms=MD2, DSA, RSA keySize < 2048
#
#
jdk.certpath.disabledAlgorithms=MD2, MD5, SHA1 jdkCA & usage TLSServer, \
    RSA keySize < 1024, DSA keySize < 1024, EC keySize < 224, \
    include jdk.disabled.namedCurves

#
# Legacy algorithms for certification path (CertPath) processing and
# signed JAR files.
#

# The syntax is the same as the "jdk.certpath.disabledAlgorithms" and
# "jdk.jar.disabledAlgorithms" security properties.
#
# Note: This property is currently used by the JDK Reference
# implementation. It is not guaranteed to be examined and used by other
# implementations.

jdk.security.legacyAlgorithms=SHA1, \
    RSA keySize < 2048, DSA keySize < 2048

#
# Algorithm restrictions for signed JAR files
#
# In some environments, certain algorithms or key lengths may be undesirable
# for signed JAR validation.  For example, "MD2" is generally no longer
# considered to be a secure hash algorithm.  This section describes the
# mechanism for disabling algorithms based on algorithm name and/or key length.
# JARs signed with any of the disabled algorithms or key sizes will be treated
# as unsigned.

# For example, the cipher suite TLS_RSA_WITH_AES_128_CBC_SHA uses RSA as the
# key exchange algorithm, AES_128_CBC (128 bits AES cipher algorithm in CBC
# mode) as the cipher (encryption) algorithm, and SHA-1 as the message digest
# algorithm for HMAC.
#
# The LegacyAlgorithm can be one of the following standard algorithm names:
#     1. JSSE cipher suite name, e.g., TLS_RSA_WITH_AES_128_CBC_SHA
#     2. JSSE key exchange algorithm name, e.g., RSA
#     3. JSSE cipher (encryption) algorithm name, e.g., AES_128_CBC
#     4. JSSE message digest algorithm name, e.g., SHA
#
# See SSL/TLS specifications and "Java Cryptography Architecture Standard
# Algorithm Name Documentation" for information about the algorithm names.
#
# Note: This property is currently used by the JDK Reference implementation.
# It is not guaranteed to be examined and used by other implementations.
# There is no guarantee the property will continue to exist or be of the
# same syntax in future releases.
#
# Example:
#   jdk.tls.legacyAlgorithms=DH_anon, DES_CBC, SSL_RSA_WITH_RC4_128_MD5
#
jdk.tls.legacyAlgorithms= \
        K_NULL, C_NULL, M_NULL, \
        DH_anon, ECDH_anon, \
        RC4_128, RC4_40, DES_CBC, DES40_CBC, \
        3DES_EDE_CBC

# The pre-defined default finite field Diffie-Hellman ephemeral (DHE)
# parameters for Transport Layer Security (SSL/TLS/DTLS) processing.
#

# For AlgConstraint, Uri is the algorithm URI String that is not allowed.
# See the XML Signature Recommendation for more information on algorithm
# URI Identifiers. For KeySizeConstraint, KeyAlg is the standard algorithm
# name of the key type (ex: "RSA"). If the MaxTransformsConstraint,
# MaxReferencesConstraint or KeySizeConstraint (for the same key type) is
# specified more than once, only the last entry is enforced.
#
# Note: This property is currently used by the JDK Reference implementation. It
# is not guaranteed to be examined and used by other implementations.
#
jdk.xml.dsig.secureValidationPolicy=\
    disallowAlg http://www.w3.org/TR/1999/REC-xslt-19991116,\
    disallowAlg http://www.w3.org/2001/04/xmldsig-more#rsa-md5,\
    disallowAlg http://www.w3.org/2001/04/xmldsig-more#hmac-md5,\
    disallowAlg http://www.w3.org/2001/04/xmldsig-more#md5,\
    maxTransforms 5,\
    maxReferences 30,\
    disallowReferenceUriSchemes file http https,\
    minKeySize RSA 1024,\
    minKeySize DSA 1024,\
    minKeySize EC 224,\
    noDuplicateIds,\
    noRetrievalMethodLoops

```

### SNMP Setup
#### Enabling SSH Access on ESXi

SSH access on an ESXi host is needed to run ESXCLI commands on a host remotely. In order to enable SSH access to your ESXi host, you can use VMware Host Client. Open a web browser, enter the IP address of your ESXi host in the address bar, then enter credentials to log in.

In the **Navigator** pane, go to **Host > Manage** and click the **Services** tab.

Right-click **TSM-SSH** and, in the context menu, click **Start.**

On the screenshot below you see the started SSH server service on the ESXi host.

[![Starting the SSH server on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-SSH-server-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-SSH-server-on-an-ESXi-host.jpg)

Now you can connect to the ESXi host from a machine with an SSH client installed. If you’re using Windows, you can use PuTTY, a free and convenient SSH client. In Linux, run the SSH client from the command line with the command:

**ssh your_username@host_ip_address**

Enter the IP address of your ESXi host and port TCP 22 (the default port number) in the session settings of the SSH client to connect to the ESXi host via SSH.

[![Connecting to the host via SSH to enable SNMP on ESXi](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Connecting-to-the-host-via-SSH-to-enable-SNMP-on-ESXi.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Connecting-to-the-host-via-SSH-to-enable-SNMP-on-ESXi.png)

#### ESXi SNMP Configuration

Once SSH access to the ESXi host is established, you can configure VMware ESXi SNMP options. On ESXi hosts, SNMP can be configured only in [the command-line interface](https://www.nakivo.com/blog/most-useful-esxcli-esxi-shell-commands-vmware-environment/). The graphical user interface (GUI) allows you only to start, stop, and restart the SNMP service.

Run the command in the console (terminal) and check the SNMP status on the ESXi host:

``` 
esxcli system snmp get
```

SNMP is disabled by default. The output for disabled SNMP on ESXi is shown on the screenshot. Most of the parameters are empty and or not configured.

[![Checking ESXi SNMP status](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Checking-ESXi-SNMP-status.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Checking-ESXi-SNMP-status.png)

##### Configuring parameters of an SNMP agent

Set SNMP parameters for an SNMP agent on the ESXi host. The SNMP agent is used to send notifications (SNMP traps and informs) to a monitoring server and receive GET, GETNEXT, and GETBULK requests.

Set the community name (“_public_” is the community name set by default). The community name in this example is “_nakivo_”.

```
esxcli system snmp set –-communities nakivo
```

Set the SNMP target. The SNMP target is a server on which monitoring software is installed to handle SNMP traps and collect monitoring information. In my example, the SNMP target is the machine running Ubuntu Linux (_192.168.101.209_). UDP 161 is the default port used for SNMP and this port is defined in my ESXi SNMP configuration:

```
esxcli system snmp set –-targets=192.168.101.209@161/nakivo
```

Specify a location, for example, the geographical location, address, datacenter, or a room where the server is located:

```
esxcli system snmp set –-syslocation “Server room”
```

Specify contact information. The system administrator’s email address can be defined for this parameter:

**esxcli system snmp set –-syscontact michaelbose@nakivo.com**

Enable SNMP on ESXi:

```
esxcli system snmp set --enable true
```

Check the SNMP status on the ESXi host again:

```
esxcli system snmp get
```

Now you can see that the parameters are configured.

[![ESXi SNMP status is enabled](https://www.nakivo.com/blog/wp-content/uploads/2021/11/ESXi-SNMP-status-is-enabled.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/ESXi-SNMP-status-is-enabled.png)

The Engine ID is the unique identifier for the SNMP agent (used for SNMP v3). The Engine ID can be set with the command (optional):

**esxcli system snmp set --engineid 544a33209458**

SNMP status is _running_ now. You can also open VMware Host Client, go to **Host > Manage > Services**, and check the status of the **snmpd** service.

[![Starting the ESXi SNMP service on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-ESXi-SNMP-service-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Starting-the-ESXi-SNMP-service-on-an-ESXi-host.jpg)

Test current SNMP configuration.

**esxcli system snmp test**

[![Testing VMware ESXi SNMP configuration](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Testing-VMware-ESXi-SNMP-configuration.png)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Testing-VMware-ESXi-SNMP-configuration.png)

If you edit SNMP settings after that, restart the SNMP agent with the command:

```
/etc/init.d/snmpd restart
```

As an alternative, you can restart ESXi SNMP in the VMware Host Client GUI in the **Services** tab. Right-click the service and click **Restart** in the context menu.

[![Restarting the VMware SNMP server service on an ESXi host](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Restarting-the-VMware-SNMP-server-service-on-an-ESXi-host.jpg)](https://www.nakivo.com/blog/wp-content/uploads/2021/11/Restarting-the-VMware-SNMP-server-service-on-an-ESXi-host.jpg)

If you need to reset ESXi SNMP settings, use the command:

**esxcli system snmp set -r**

The command to disable SNMP on an ESXi host is:

**esxcli system snmp set –enable false**
