# pfBlockerNG
pfBlockerNG is created, designed, developed, supported and maintained by: BBcan177

-   [http://pfblockerng.com/](http://pfblockerng.com/)
-   [https://www.reddit.com/r/pfBlockerNG/new/](https://www.reddit.com/r/pfBlockerNG/new/)
-   [https://github.com/BBcan177](https://github.com/BBcan177)

It is an amazing package that allows you to block and filter dns requests and ips. Please support him

Here is a video walkthrough of how to get started:

-   [https://www.youtube.com/watch?v=xizAeAqYde4](https://www.youtube.com/watch?v=xizAeAqYde4)

I have an extensive block list as you will see. you do not need to be as granular as I am.

[![pfBlocker_1](https://user-images.githubusercontent.com/12887622/134786449-3e6b49d2-a18b-44e8-96d2-906dea00115f.JPG)](https://user-images.githubusercontent.com/12887622/134786449-3e6b49d2-a18b-44e8-96d2-906dea00115f.JPG)


# [pfBlockerNG Guide by sunnyvalley](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng)

`pfBlockerNG` is an excellent Free and Open Source package developed for pfSense® software that provides advertisement blocking and malicious content blocking, as well as geo-blocking capabilities.

By installing pfBlockerNG, you can not only block ads but also web tracking, malware and ransomware. When you use pfBlockerNG, you gain extra security and privacy. It will do this for your entire network by utilizing a feature known as `DNSBL` (short for Domain Name System-based Blackhole List). pfBlockerNG also allows you to block internet traffic from specific IP addresses. These IP addresses may belong to specific countries and regions, which can be very useful in protecting your network from all of those hackers attempting to gain access to it.

TIP

If you want to also add **Next Generation Firewall** capabilities to your **open source firewall**, check out [Zenarmor (previously Sensei)](https://www.sunnyvalley.io/product). Zenarmor is a plug-in that upgrades your open source firewall to a NGFW _in a matter of seconds_.

Some of the available features are: _[Application](https://www.sunnyvalley.io/docs/policies/application-control-rules)/[User based blocking](https://www.sunnyvalley.io/docs/guides/user-based-filtering-using-opnsense-captive-portal), [Web/Content Filtering](https://www.sunnyvalley.io/docs/policies/web-control-rules), Enterprise-grade Network Analytics, Policy-based filtering, Ad Blocking, Real-time [Cloud Threat Intelligence](https://www.sunnyvalley.io/docs/opnsense/configuring/cloud-threat-intelligence), [Active Directory Integration](https://www.sunnyvalley.io/docs/opnsense/configuring/ad-integration), Cloud-managed central policies_ and many more.

You can [install](https://www.sunnyvalley.io/docs/installing/installation) and start to use [Zenarmor Free Edition](https://www.sunnyvalley.io/free-edition-plan) forever on your pfSense® software firewall.

## What is pfBlockerNG?[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#what-is-pfblockerng "Direct link to heading")

pfBlockerNG is a pfSense® software package created by `BBCan177` and used for IP/DNS-based filtering. It is based on the previous work of Marcello Coutinho and Tom Schaefer. The project's goal was to extend pfSense's core firewall functionality by allowing users to control and manage inbound and outbound access through the firewall using IP and DNS control lists.

pfBlockerNG gives pfSense® software the ability to make allow/deny decisions based on items like the geolocation of an IP address, the domain name of a resource, or the Alexa ratings of specific websites.

Most of the pfSense® software users think that pfBlockerNG is a fantastic package and a pfSense® installation would be incomplete without it.

## History of pfBlockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#history-of-pfblockerng "Direct link to heading")

Since 2014, pfBlockerNG has been protecting assets behind pfSense® software consumer and corporate networks. The desire to create a unified solution to manage IP and Domain feeds with rich customization and management features drove the development of pfBlockerNG. BBcan177 an independent developer created, designed, and developed pfBlockerNG. It is still being supported and maintained by BBcan177.

Before pfBlockerNG was born, the pf-blocker developed by Marcello Coutinho was widespread among the pfSense® community. Pf-blocker was the successor of the `Country Block` developed by Tom Schaefer. On Oct 27, 2011, Country Block ended and the `pf-blocker` took over. The package was designed to keep a mail server from being flooded with spam. However, pf-blocker was unable to process the required feeds, and when large IP feeds were added, it crashed. BBcan177 had offered to assist the developer in adding some additional functionality, but he got nothing in return. As a result, Pf-blocker life was very short and the last commit to the pf-blocker GitHub repository was on Jun 20, 2014. Fortunately, pfBlockerNG was released on Nov 30, 2014, and pf-blocker ended.

BBcan177 takes a lot of responsibility for developing pfBlockerNG and making sure that it is thoroughly tested before release and that any issues are resolved as soon as possible.

It's worth noting that BBCan177 has a Patreon campaign where you can easily donate a few dollars to ensure he keeps up with and improves the package. We strongly encourage you to donate if you are using pfBlockerNG in a production environment.

At the time of writing this article, the latest version of pfBlockerNG-devel package is v3.0.0_16 released on April 8th of 2021.

## Features of the pfBlockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#features-of-the-pfblockerng "Direct link to heading")

pfBlockerNG includes a wide variety of features such as country blocking, IP/DNS blacklisting, and IP reputation blocking to protect your network from unwanted traffic. We will cover the pfBlockerNG features briefly below.

### IP Blocking[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ip-blocking "Direct link to heading")

pfBlockerNG allows you to create firewall rules based on IPv4 and IPv6 address spaces. So that You can control both incoming and outgoing traffic on single or multiple interfaces. You can also restrict the IP address according to geolocation. `Geolocation` is the identification or estimation of an IP address's real-world geographic location. `MaxMind`, an industry leader in the accuracy of IP geolocation provides and maintains lists that are used by pfBlockerNG. Websites host content and media on servers all over the world, so be cautious about blocking too much. Inadvertently blocking some of these IP addresses may result in broken websites or unavailable downloads.

### DNS Blocking[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#dns-blocking "Direct link to heading")

pfBlockerNG can also control DNS Resolver access to prevent access to malicious websites such as advertisements, threats, and malware. [DNS filtering](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-dns-filtering) is an effective method to filter tracking domains, malicious domains, and advertisements. Your DNS requests are checked against a blocklist as you browse the internet. If a match is found, the request is denied. It's an excellent way to block ads without using a proxy server.

Domain names gathered from various blacklist sources or manually entered are used to generate optimized DNS Resolver blocklists. You can subscribe to popular user-maintained blocklists as well as use prebuilt `EasyLists`.

INFO

The `EasyList` filter lists are sets of rules originally designed for `Adblock` that automatically remove unwanted content from the internet, such as irritating advertisements, bothersome banners, and inconvenient tracking. It is the most commonly used list by many ad blockers and serves as the foundation for over a dozen combination and supplementary filter lists.

BEST PRACTICE

DNS filtering applications have some weaknesses, such as DNS Evasion, Poor of Manageability/Portability/Flexibility/Reporting and Analytics, Recent Website-based Attacks.

Therefore, DNS filtering solutions doesn't provide complete network security on their own; instead, they should be used in concert with next-generation firewalls as an additional layer of defense in accordance with the [defense-in-depth](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-defense-in-depth) approach. They should never be viewed as a high-level security mechanism.

### Inbound traffic filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#inbound-traffic-filtering "Direct link to heading")

pfSense® software blocks all inbound traffic by default. Therefore, there is no need to apply a rule to inbound traffic for additional protection unless there are open ports on your firewall. However, you may occasionally have a number of ports open, exposing a [VPN](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-vpn) endpoint and several self-hosted services. If this is the case, then it is advisable to use the custom IP list and GeoIP restriction features of pfBlockerNG to limit access.

### Outbound traffic filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#outbound-traffic-filtering "Direct link to heading")

Outbound blocking is available in pfBlockerNG to prevent users from accidentally visiting malicious websites. When combined with logging, this is a useful method for identifying potentially compromised devices.

### Policy-based routing[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#policy-based-routing "Direct link to heading")

pfBlockerNG allows you to create policy-based routing firewall rules that direct traffic away from specific gateways or gateway groups.

### Malicious DNS Blocking and advert limiting[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#malicious-dns-blocking-and-advert-limiting "Direct link to heading")

DNS blocking to networks served by the DNS Resolver is also supported in pfBlockerNG to prevent access to tracking and/or malicious sites. Be cautious of the possibility of introducing false positives.

### Spam Filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#spam-filtering "Direct link to heading")

If you have a mail server on your network, pfBlockerNG is an excellent package to use. You can prevent spam from reaching your server by including a [spam](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-spam-email) blacklist, such as `Spamhaus`.

### Whitelists[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#whitelists "Direct link to heading")

If you want a domain not to be blocked, pfBlockerNG allows you to add it to the whitelist.

### SafeSearch[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#safesearch "Direct link to heading")

SafeSearch can be configured for the most popular search engines. You can also use Firefox to block DNS over HTTPS and set YouTube restrictions.

## How to Install and Configure pfBlockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#how-to-install-and-configure-pfblockerng "Direct link to heading")

You can easily set up and configure the pfBlockerNG package on your pfSense® software firewall by following these steps:

1.  pfBlockerNG package installation
2.  pfBlockerNG initial configuration

### pfBlockerNG package installation[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#pfblockerng-package-installation "Direct link to heading")

To install the pfBlockerNG package, you may follow the instructions given below.

1.  Access your pfSense® software WebGUI.

![pfSense® Software CE GUI sign in page](https://www.sunnyvalley.io/docs/assets/images/Figure-1-pfSense%C2%AESoftwareCEGUIsigninpage-576ab320e945df418149facd07011ace.png "pfSense® Software CE GUI sign in page")

**Figure 1.** _pfSense® Software CE GUI sign-in page_

INFO

Default username and password for pfSense® software is `admin` and `pfsense`. It is strongly recommended that you change your password with a strong one.

2.  Navigate to the `System` -> `Package Manager`->`Available Packages`.

![Accessing Package Manager on pfSense® Software CE GUI](https://www.sunnyvalley.io/docs/assets/images/Figure-2-AccessingPackageManageronpfSense%C2%AESoftwareCEGUI-636bc83c3b5bcc6b447a6e907b31aa48.png "Accessing Package Manager on pfSense® Software CE GUI")

**Figure 2.** _Accessing Package Manager on pfSense® Software CE GUI_

![Accessing Available Packages on pfSense® Software CE GUI](https://www.sunnyvalley.io/docs/assets/images/Figure-3-AccessingAvailablePackagesonpfSense%C2%AESoftwareCEGUI-26f382749196e68f5fc9862f5f961d35.png "Accessing Available Packages on pfSense® Software CE GUI")

**Figure 3.** _Accessing Available Packages on pfSense® Software CE GUI_

3.  Type `pfblockerng` into the search field and then click `search`.
4.  Click `install` on the version with `-devel` at the end of the package.

![Search and install pfBlockerNG-devel package](https://www.sunnyvalley.io/docs/assets/images/Figure-4-SearchandinstallpfBlockerNG-develpackage-83531dd2b971cdb8af76a95d9f9f75ae.png "Search and install pfBlockerNG-devel package")

**Figure 4.** _Search and install pfBlockerNG-devel package_

5.  Click `Confirm` to let the package install. This will take some time because it needs to download several files and databases.

![Confirmation for installing pfBlockerNG-devel package](https://www.sunnyvalley.io/docs/assets/images/Figure-5-ConfirmationforinstallingpfBlockerNG-develpackage-8190830e2aecd0921e63ddf8572d761a.png "Confirmation for installing pfBlockerNG-devel package")

**Figure 5.** _Confirmation for installing pfBlockerNG-devel package_

6.  Once the installation is complete, you should see `success` after a few minutes.

![pfBlockerNG-devel package installation completed succesfully](https://www.sunnyvalley.io/docs/assets/images/Figure-6-pfBlockerNG-develpackageinstallationcompletedsuccesfully-96eb66f13b90698b82c2ba9caf2a0d9b.png "pfBlockerNG-devel package installation completed succesfully")

**Figure 6.** _pfBlockerNG-devel package installation completed successfully_

### pfBlockerNG initial configuration[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#pfblockerng-initial-configuration "Direct link to heading")

1.  Click on the `Firewall` drop-down menu on your pfSense® software GUI.
2.  Click on `pfBlockerNG` to start the configuration wizard.

![Accessing pfBlocker menu on pfSense® software GUI](https://www.sunnyvalley.io/docs/assets/images/Figure-7-AccessingpfBlockermenuonpfSense%C2%AEsoftwareGUI-46b3813758e6e407211f94585c916e3b.png "Accessing pfBlocker menu on pfSense® software GUI")

**Figure 7.** _Accessing pfBlocker menu on pfSense® software GUI_

3.  Click `Next` to continue.

![pfBlockerNG setup wizard](https://www.sunnyvalley.io/docs/assets/images/Figure-8-pfBlockerNGsetupwizard-0b633e67240d8a0baa84dd4449436c34.png "pfBlockerNG setup wizard")

**Figure 8.** _pfBlockerNG setup wizard_

4.  Click `Next` to proceed to the configuration. This will remove all settings if you have previously configured pfBlockerNG and install the following components:

-   **IP:** Firewall rules will be defined for the WAN interface to block the worst-known attackers.
-   **DNSBL:** DNS resolver will be utilized so that advertising and other known malicious domains are blocked.

![pfBlockerNG component installation notice](https://www.sunnyvalley.io/docs/assets/images/Figure-9-pfBlockerNGcomponentinstallationnotice-b0fa072634f2e2eca75b0e106ccf3640.png "pfBlockerNG component installation notice")

**Figure 9.** _pfBlockerNG component installation notice_

5.  Select `[WAN](/docs/network-basics/what-is-wan)` for `Inbound Firewall Interface` and `[LAN](/docs/network-basics/what-is-lan)` for `Outbound Firewall Interface` to complete the IP Component Configuration. If you have more than one internal interface, you may select all the ones you wish to set up pfBlockerNG for.

![pfBlockerNG IP Component Configuration](https://www.sunnyvalley.io/docs/assets/images/Figure-10-pfBlockerNGIPComponentConfiguration-7cb4866885c04d776f23394ef6da151a.png "pfBlockerNG IP Component Configuration")

**Figure 10.** _pfBlockerNG IP Component Configuration_

6.  Click on `Next` to proceed to the configuration.
7.  Enter an IP address that is not used in your networks for `VIP address` and leave the `port` and `ssl port` as default. pfBlockberNG DNSBL web server will run on these IP addresses. If your LAN is 10.1.1.0/24, the VIP address should not be in this range. Here in our example, we leave the address at 10.10.10.1. Also, you may enable IPv6 DNSBL and DNSBL Whitelist options.

![pfBlockerNG DNSBL Component Configuration](https://www.sunnyvalley.io/docs/assets/images/Figure-11-pfBlockerNGDNSBLComponentConfiguration-b5f785fe2ddc518b21571ad67ae27820.png "pfBlockerNG DNSBL Component Configuration")

**Figure 11.** _pfBlockerNG DNSBL Component Configuration_

8.  Click on `Finish` to finish the wizard. The setup is now complete.

![pfBlockerNG initial configuration finalize](https://www.sunnyvalley.io/docs/assets/images/Figure-12-pfBlockerNGinitialconfigurationfinalize-bc8ab81b2cf70eed3ec8666d362e2faf.png "pfBlockerNG initial configuration finalize")

**Figure 12.** _pfBlockerNG initial configuration finalize_

9.  The pfBlockerNG update page then appears, and all activated blocklists are automatically downloaded and activated. Also, you may select the `Cron` option for regular updates.

![pfBlockerNG update settings](https://www.sunnyvalley.io/docs/assets/images/Figure-13-pfBlockerNGupdatesettings-4cd0df375d163134eb087c2a83b77a00.png "pfBlockerNG update settings")

**Figure 13.** _pfBlockerNG update settings_

Congratulations! You now have a basic pfSense® web filter running with pfblockerNG!

![pfBlockerNG installation is complete](https://www.sunnyvalley.io/docs/assets/images/Figure-14-pfBlockerNGinstallationiscomplete-a3e884078af831602c280aeb60b27123.png "pfBlockerNG installation is complete")

**Figure 14.** _pfBlockerNG installation is complete_

### General Settings of pfBlockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#general-settings-of-pfblockerng "Direct link to heading")

To view or change the general settings of the pfBlockerNG, you may navigate to `Firewall`-> `pfBlockerNG ->` General`.

Make sure that pfBlockerNG is enabled on your pfSense® software firewall. You may leave the settings on this page at their default values.

![General Settings of pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-15-GeneralSettingsofpfBlockerNG-36cd9cd97d95726902af8a7e31881f3d.png "General Settings of pfBlockerNG")

**Figure 15.** _General Settings of pfBlockerNG_

## IP Filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ip-filtering "Direct link to heading")

Even if the firewall is not configured with open internet facing ports, local users may inadvertently initiate connections to malicious servers and this may be a high-security risk for your network. To reduce the likelihood of this happening, you should restrict access to known sources of [Ransomware](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-malware#9--ransomware), [malware](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-malware), botnets, and Command & Control (C&C) servers. Through the bundled PRI1 feed, pfBlockerNG provides regularly updated blocklists.

In this section, we'll explain how to enable the IP feed (PRI1-PR5 groups) on pfBlockerNG and set up a firewall rule to prevent outbound traffic from accessing any addresses in that group.

#### IP Configuration[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ip-configuration "Direct link to heading")

You should navigate to the `Firewall`-> `pfBlockerNG ->` IP`and ensure the following settings on`IP Configuration` pane.

1.  **Enable De-Duplication.** This option provides reducing the list size by detecting and removing duplicate entries
2.  **Enable CIDR aggregation.** This option optimizes CIDRs. Because CIDR aggregation is processor intensive, you may need to disable it if your firewall does not have enough power.
3.  **Enable Suppression.** When enabled, RFC1918 and loopback addresses are filtered. Suppression makes sure that your local subnets are not blocked. Also, pfBlockerNG removes any deny list entries that match those specified in the Suppression list which can be manually or automatically populated from the pfBlockerNG alerts tab.
4.  You may leave other settings as default. But, ensure that the Placeholder IP address is not used in your network. Also, you may enable ASN reporting, When it is enabled the Alerts and Statistics tab will report the ASN for the Block/Reject/Permit/Match IP entries. The ASN details are collected from BGPview.io and cached for 1 week (_can be configured for 24,12,4,1 hour caching_)

![IP Configuration pane of pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-16-IPConfigurationpaneofpfBlockerNG-811e05436013db4c035bcf700d8f9e02.png "IP Configuration pane of pfBlockerNG")

**Figure 16.** _IP Configuration pane of pfBlockerNG_

5.  Click `Save IP Settings` button at the end of the page

#### MaxMind GeoIP configuration[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#maxmind-geoip-configuration "Direct link to heading")

With pfBlockerNG's GeoIP feature, you can filter traffic to and from entire countries or continents. pfBlockerNG accomplishes this by utilizing the MaxMind GeoIP database, which requires a license key. This license key is completely free. The MaxMind License Key field description includes a link to the MaxMind registration page.

To obtain your license key, fill out the registration form on the MaxMind sign-up page.

![MaxMind GeoLite2 Sign Up page](https://www.sunnyvalley.io/docs/assets/images/Figure-17-MaxMindGeoLite2SignUppage-623e49cae3a3b5ccb2e0b1732a2b8f1e.png "MaxMind GeoLite2 Sign Up page")

**Figure 17.** _MaxMind GeoLite2 Sign Up page_

![MaxMind Managing license keys](https://www.sunnyvalley.io/docs/assets/images/Figure-18-MaxMindManaginglicensekeys-dd55d00d7ad5e7c30fa4fcef7bb79ec7.png "MaxMind Managing license keys")

**Figure 18.** _MaxMind Managing license keys_

After generating a license key, enter it in the MaxMind License Key field on the pfBlockerNG.

You may select MaxMind localized language as you wish. The following languages are available:

-   English
-   French
-   Brazilian Portuguese
-   Spanish
-   German
-   Japanese
-   Simplified Chinese

Also, you may disable the MaxMind monthly CSV GeoIP database cron update.

![MaxMind GeoIP configuration](https://www.sunnyvalley.io/docs/assets/images/Figure-19-MaxMindGeoIPconfiguration-71db4b62553d802ffa53cdd50de06cb1.png)

**Figure 19.** _MaxMind GeoIP configuration_

#### IPv4 Suppression List[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ipv4-suppression-list "Direct link to heading")

pfBlockerNG allows you to add the IP addresses (only for /32 or /24) that should never be blocked to the suppression list. You can add one IP address per line. You must run `Force Reload-IP` after manually adding an IP address to this list, for changes to take effect.

![IPv4 Suppression list](https://www.sunnyvalley.io/docs/assets/images/Figure-20-IPv4Suppressionlist-fa66a9b6ed6ace3bddacc1afd0e5b204.png "IPv4 Suppression list")

**Figure 20.** _IPv4 Suppression list_

#### IP Interface/Rules Configuration[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ip-interfacerules-configuration "Direct link to heading")

According to the settings in the `IP Interface/Rules Configuration` pane, pfBlockerNG defines firewall rules automatically. In this pane, you can specify which inbound and outbound interface(s) pfBlockerNG's IPv4, IPv6, and GeoIP filtering apply to. To determine the inbound and outbound interfaces you may follow the next instructions.

1.  Select `WAN` for Inbound Firewall Rules to apply auto rules to the inbound interface.
2.  Select `LAN` for Outbound Firewall Rules to apply auto rules to the outbound interface.
3.  Enabling the Floating Rules option may be useful if you have more than one outbound interface. Floating rules are special firewall rules that take precedence over regular firewall rules. This ensures that pfBlockerNG begins filtering traffic as soon as it enters the firewall. Another advantage is that pfBlockerNG will generate the floating rules for you.
4.  Enable `Kill states`. Since IP blocklists are updated several times per day and you should allow pfBlockerNG to immediately kill any connection to a blocked IP.
5.  You may leave other options as default.
6.  Click on the `Save IP Settings` button at the bottom of the page.

![IP Interface/Rules Configuration on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-21-IPInterface-RulesConfigurationonpfBlockerNG-d590e72a9883dfc087262c1d7dae11f6.png "IP Interface/Rules Configuration on pfBlockerNG")

**Figure 21.** _IP Interface/Rules Configuration on pfBlockerNG_

### Enabling IPv4 Filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#enabling-ipv4-filtering "Direct link to heading")

On pfBlockerNG PRI1 feed is enabled by default. Feeds are publicly available blocklists that pfBlockerNG is configured to synchronize with on a regular basis. To view the list of enabled IPv4 feeds, navigate to the `Firewall` -> `pfBlockerNG` -> `IP` -> `IPv4`.

![Enabled IPv4 feed on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-22-EnabledIPv4feedonpfBlockerNG-b1e7f63fb71c16d2d83dfa240bd75d3a.png "Enabled IPv4 feed on pfBlockerNG")

**Figure 22.** _Enabled IPv4 feed on pfBlockerNG_

PRI1 feed has a fairly broad coverage but is designed to avoid false positives, so there is a greater chance that it will miss genuine threats. To harden the security on your network, you should enable additional IPv4 feeds on your pfBlockerNG. To view the list of available feeds on the pfBlockerNB, navigate to the `Firewall` -> `pfBlockerNG` -> `Feeds`.

![IPv4 Category feeds](https://www.sunnyvalley.io/docs/assets/images/Figure-23-IPv4Categoryfeeds-e78d4ab90d12dbba622f62895570ca15.png "IPv4 Category feeds")

**Figure 23.** _IPv4 Category feeds(PRI1-5)_

At the time of writing, the available `Number of Feeds per Category Type` is given below:

Category

Number of Feeds

IPv4

92

IPv6

14

DNSBL

140

**Table 1.** _Number of Feeds per Category Type_

IPv4 Category feeds are divided into five groups(PRI1-5). These PRI groups are Known Ransomware, malware, botnets, Command & Control (C&C) servers, [bots](https://www.sunnyvalley.io/docs/network-security-tutorials/what-is-malware#8--bots), web scripts, [phishing](https://www.sunnyvalley.io/docs/network-security-tutorials/what-are-phishing-attacks) & compromised servers, malicious IP's found attacking SSH, SMTP, IMAP, TELNET, FTP endpoints and other known originators of malicious behavior. In general, the lower the number, the more pfBlockerNG tries to avoid false positives. Therefore you should be prepared for some websites to be unreachable unexpectedly if you enable the more restrictive lists (PRI3 and above). In such cases, some troubleshooting and possibly whitelisting of false positives will be required. There are also a variety of feed groups aimed at blocking specific types of malicious or undesirable traffic such as:

-   Scanner (Internet Storm Center)
-   Mail (Known sources of spam; useful for protecting mail servers)
-   Forum Spam
-   Tor nodes(Known Tor exit points; not inherently dangerous but you may want to isolate users anonymizing their traffic.)
-   Internic (Contains root name servers needed to initialize the cache of Internet domain name servers)
-   Proxy IP
-   Torrent IP
-   Public DNS
-   DOH (DNS over HTTP)
-   VPN
-   BlocklistDE

![Other IPv4 Category feed groups](https://www.sunnyvalley.io/docs/assets/images/Figure-24-OtherIPv4Categoryfeedgroups-3ca4c2dbe64cd3a118fad578e25e45f6.png "Other IPv4 Category feed groups")

**Figure 24.** _Other IPv4 Category feed groups_

You may enable IPv4 category PRI3 group feeds on your pfBlockerNG by following the next steps.

1.  Scroll down to the PRI3 group header and click the + icon next to the group name. This will redirect you to the settings page to add the rule.

![Adding IPv4 category PRI3 group feeds](https://www.sunnyvalley.io/docs/assets/images/Figure-25-AddingIPv4categoryPRI3groupfeeds-e078ee86af385916ade393de14c8c282.png "Adding IPv4 category PRI3 group feeds")

**Figure 25.** _Adding IPv4 category PRI3 group feeds_

2.  You may set the name and description, or leave them as default.
3.  Select `ON` option in the `State` drop-down menu for all feeds in the `IPv4 Source Definitions` pane. You may also select `HOLD` option if you wish to download the list once but exclude it from automatic updates. We will not enable the `BBC_C2` feed as it requires an API key.
4.  You may also click the `Enable All` button at the bottom of the `IPv4 Source Definitions` pane to enable all feeds.

![IPv4 source definitions for PRI3 group on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-26-IPv4sourcedefinitionsforPRI3grouponpfBlockerNG-aa47699c77a80683038d979ae85b7ca5.png "IPv4 source definitions for PRI3 group on pfBlockerNG")

**Figure 26.** _IPv4 source definitions for PRI3 group_

5.  Scroll down to the `Settings` pane and select one of the `Action` options you wish to take when an IP address is matched.
6.  Select `Deny Both` in the `Action` drop-down menu to apply the rule to both inbound and outbound connections.

![IPv4 category settings to add PRI3 feeds on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-27-IPv4categorysettingstoaddPRI3feedsonpfBlockerNG-a296046e299883bb4d9283b1502faa26.png "IPv4 category settings to add PRI3 feeds on pfBlockerNG")

**Figure 27.** _IPv4 category settings to add PRI3 feeds on pfBlockerNG_

7.  Leave other settings as default.
8.  Click on the `Save IPv4 Settings` button.
9.  Congratulations! You have successfully enabled IPv4 category PRI3 feeds on your pfBlockerNG to protect your network.
10.  You may also apply PRI feeds rule to both inbound and outbound connections by selecting `Deny Both` in Action drop-down menu and clicking the `Save` button on `IPv4 Summary` pane.

![IPv4 category settings](https://www.sunnyvalley.io/docs/assets/images/Figure-28-IPv4categorysettings-3a4ac0f9fb44b3dc98ca4cac8a3c9acc.png)

**Figure 28.** _IPv4 category settings_

You can follow the similar steps given above for enabling other PRI groups, IPv6 and DNS blocklists, just add the alias group, select the lists you want to enable, and choose the action to be taken when an item is matched. However, be aware that there is a memory and processing impact with each list enabled and you may overload your hardware.

### Verifying IPv4 Filtering[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#verifying-ipv4-filtering "Direct link to heading")

By following the given steps below you may verify IPv4 filtering on your pfBlockerNG. Before starting to test IPv4 filtering you should ensure that pfBlockerNG settings are updated. If it is not, you may Force Update by clicking on the `Run` button in the `Update Settings` under `Update` tab of the pfBlockerNG.

1.  Navigate to the `Firewall` -> `Rules` -> `Floating`.
2.  Ensure that the firewall rules for blocking IPv4 category PRI3 groups are added.

![Firewall floating rules on pfSense® software for blocking IPv4 category PRI3 groups](https://www.sunnyvalley.io/docs/assets/images/Figure-29-FirewallfloatingrulesonpfSense%C2%AEsoftwareforblockingIPv4categoryPRI3groups-9b7a7be34510705731af7df69abb79f9.png "Firewall floating rules on pfSense® software for blocking IPv4 category PRI3 groups")

**Figure 29.** _Firewall floating rules on pfSense® software for blocking IPv4 category PRI3 groups_

3.  Hover your mouse over the Source `pfB_PRI3_v4` to view the blocked IP lists.

![Viewing IPv4 PRI3 alias details](https://www.sunnyvalley.io/docs/assets/images/Figure-30-ViewingIPv4PRI3aliasdetails-51f5fbacbce70626e59e3ff6102d2f43.png "Viewing IPv4 PRI3 alias details")

**Figure 30.** _Viewing IPv4 PRI3 alias details_

4.  Note one of the IP addresses from the list to try to access for testing IPv4 filtering. We will select `1.0.221.21` for testing
5.  You may open your browser and enter the IP address you select from the list to the search bar or ping the IP address from the CLI prompt. You should see that the IP address is not reachable.

![PRI3 ip address is not reachable](data:image/png;base64,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 "PRI3 ip address is not reachable")

**Figure 31.** _PRI3 ip address is not reachable_

6.  To view that IP address is blocked by pfBlockerNG you may check the related firewall logs click on the `Related log entries` icon at the top right corner of the page.
7.  Search for the IP address that tries to access, such as `1.0.221.21`. You should see the related logs showing the PRI3 IP address is blocked by pfBlockerNG as given in the figure below.

![Firewall log showing PRI3 ip address is blocked by pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-32-FirewalllogshowingPRI3ipaddressisblockedbypfBlockerNG-92b762f8cd30368f01ceac9088ad376b.png "Firewall log showing PRI3 ip address is blocked by pfBlockerNG")

**Figure 32.** _Firewall log showing PRI3 ip address is blocked by pfBlockerNG_

## GeoIP Blocking[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#geoip-blocking "Direct link to heading")

GeoIP feature of the pfBlockerNG can be useful for restricting access to specific regions. This will not be useful in all circumstances because not all regions are malicious. However, if all of your expected traffic comes from a specific geographic region, allowing traffic from other regions is pointless because it exposes you to additional risk for no real benefit. In most cases, you'll only need to block inbound access based on GeoIP data. This allows your local users to access any websites all over the world while blocking inbound access from regions where you don't expect traffic.

To enable GeoIP Blocking on your pfBlockerNG,

1.  Navigate to the `Firewall` -> `pfBlockerNG` -> `IP` -> `GeoIP`.
2.  Select `Deny Inbound` in `Action` drop-down menu for `Top Spammers` -a list of countries that have been identified as a frequent source of online attacks- and `Proxy and Satellite` -well known anonymous proxy and satellite providers-.
3.  You may also select one of the continents where you never expect legitimate traffic to originate.

![GeoIP blocking on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-33-GeoIPblockingonpfBlockerNG-b98fae984e12104f5ffde5bbf00a9247.png "GeoIP blocking on pfBlockerNG")

**Figure 33.** _GeoIP blocking on pfBlockerNG_

4.  Click the `Save` button.

Instead of blocking a whole region, you may block specific countries. To block a country in a region;

1.  Click on the pencil icon next to the region.
2.  Select the countries that you wish to block.
3.  Enable `List Action` and `Logging`
4.  Click on `Save`.

![Blocking countries using GeoIP on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-34-BlockingcountriesusingGeoIPonpfBlockerNG-ac6407f846fbf4ab94eee06700240916.png "Blocking countries using GeoIP on pfBlockerNG")

**Figure 34.** _Blocking countries using GeoIP on pfBlockerNG_

## DNS Blocking[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#dns-blocking-1 "Direct link to heading")

You may block advertisements and some malicious sites such as Malware, Porn, Gambling, etc. by pfBlockerNG which has DNS blackholing capability. When you enable the DNSBL feature on your pfBlockerNG, the DNS requests against a list of known ad networks and trackers will be blocked at the DNS level on your network.

To be able to use the DNS Blocking feature of the pfBlockerNG, you should make sure that your client devices are configured to use the pfSense® software firewall as their DNS server. If you are using a standard [pfSense® software](https://www.sunnyvalley.io/docs/network-security-tutorials/pfsense) configuration, this will be set automatically. However, if you have configured an alternative DNS server, such as a Pi-hole, you should check the DNS configuration on pfSense® software and configure client devices to use it.

1.  Navigate to `Services` -> `DNS Resolver` -> `General Settings` and check that the DNS resolver is enabled.

![Enabling DNS resolver on pfSense® software](https://www.sunnyvalley.io/docs/assets/images/Figure-35-EnablingDNSresolveronpfSense%C2%AEsoftware-80f7b49adc0ad56563068820afcef062.png "Enabling DNS resolver on pfSense® software")

**Figure 35.** _Enabling DNS resolver on pfSense® software_

2.  Navigate to `System` -> `General Setup` and check that external DNS resolvers are configured as these will be required to forward DNS requests that aren't blocked. You may add Google DNS server, `8.8.8.8`, as external DNS and click the `Save` button.

![Adding DNS server on pfSense® software](https://www.sunnyvalley.io/docs/assets/images/Figure-36-AddingDNSserveronpfSense%C2%AEsoftware-51661f55cda7c1430b6bbde7fb893e64.png "Adding DNS server on pfSense® software")

**Figure 36.** _Adding DNS server on pfSense® software_

3.  Navigate to `Services` -> `DHCP Server` and select all the interfaces for which you want to enable blocking and ensure that nothing is listed under DNS servers. If you have a configured static DNS, set them to your pfSense® software firewall's IP address.

1.  Navigate to the `Firewall`-> `pfBlockerNG ->` IP`
2.  Enable `DNSBL`.
3.  Select `Unbound python mode` for DNSBL mode setting.

TIP

`Unbound python mode` requires substantially less memory than the `unbound mode`. It allows for some advanced options too.

4.  Ensure that the following options are enabled:

-   Wildcard Blocking TLD
-   DNS Reply Logging: This will show you all the DNS queries which are answered by Unbound.
-   DNSBL Blocking
-   HSTS mode
-   CNAME Validation checked: This option must be enabled to make sure that an ad domain cannot `bypass` DNSBL by using a different DNS name.

![DNSBL settings on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-37-DNSBLsettingsonpfBlockerNG-cc6bcf0341c838b3163e4e0bfc73f7a2.png "DNSBL settings on pfBlockerNG")

**Figure 37.** _DNSBL settings on pfBlockerNG_

5.  Scroll down to the `DNSBL Webserver Configuration` pane. Make sure that the Virtual IP address is correct and It is not already used in the Network. You may leave other settings as default.

![DNSBL webserver configuration on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-38-DNSBLwebserverconfigurationonpfBlockerNG-5267ccf849e8deeda2eef4eb01ba45c2.png "DNSBL webserver configuration on pfBlockerNG")

**Figure 38.** _DNSBL webserver configuration on pfBlockerNG_

6.  Scroll down to the `DNSBL Configuration` pane.
7.  Enable `Permit Firewall Rules` and select `LAN` interface. This will create rules in the Floating in your Firewall and enable pfBlockerNG for selected networks(LAN).
8.  Select DNSBL Webserver/VIP for Global Logging/Blocking Mode. So that Domains are sinkholed to the DNSBL VIP and logged via the DNSBL WebServer. You may leave other settings as default.

![DNSBL configuration on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-39-DNSBLconfigurationonpfBlockerNG-4eb58c88ac78a88bfb3cd0a9bfd60985.png "DNSBL configuration on pfBlockerNG")

**Figure 39.** _DNSBL configuration on pfBlockerNG_

9.  Click `Save DNSBL Settings` button at the bottom of the page.

### Enable some DNSBL feeds[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#enable-some-dnsbl-feeds "Direct link to heading")

On pfBlockerNG ADS_Basic feed is enabled by default. To view the list of enabled DNSBL feeds, navigate to the `Firewall` -> `pfBlockerNG` -> `DNSBL` -> `DNSBL Groups`.

![Enabled DNSBL Group feed on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-40-EnabledDNSBLGroupfeedonpfBlockerNG-06257e840c282a38078928ad2192aa81.png "Enabled DNSBL Group feed on pfBlockerNG")

**Figure 40.** _Enabled DNSBL Group feed on pfBlockerNG_

ADS_Basic feed, also known as StevenBlack_ADs, has a fairly broad coverage but is designed to avoid false positives, so there is a greater chance that it will miss genuine threats. To harden the security on your network, you should enable additional DNSBL feeds on your pfBlockerNG. To view the list of available feeds on the pfBlockerNB, navigate to the `Firewall` -> `pfBlockerNG` -> `Feeds`.

![DNSBL Category feeds](https://www.sunnyvalley.io/docs/assets/images/Figure-41-DNSBLCategoryfeeds-72f36a711a4bf1ec4cef4274154dcaf7.png "DNSBL Category feeds")

**Figure 41.** _DNSBL Category feeds_

At the time of writing, there are 140 DNSBL Category Feeds available. There are also a variety of feed groups on pfBlockerNG aimed at blocking specific types of malicious or undesirable traffic such as:

-   EasyList
-   ADs
-   Email
-   Malicious
-   Phishing
-   BBCAN177
-   STUN
-   DoH
-   Torrent
-   BBC
-   Malicious2
-   Cryptojackers
-   Compilation
-   Firebog_Suspicious
-   Firebog_Advertising
-   Firebog_Trackers
-   Firebog_Malicious
-   Firebog_Other

You may enable different DNSBL feeds as you wish on your pfBlockerNG by following the next steps. Here, we will enable EasyList group feeds on our pgBlockerNG as an example. We also recommend you add the Steven Black feed is one of the best-maintained blacklist databases on the internet.

INFO

EasyList is the primary filter list that removes the majority of advertisements from international webpages, as well as unwanted frames, images, and objects. It is the most commonly used list by many ad blockers and serves as the foundation for over a dozen combination and supplementary filter lists.

CAUTION

The more feeds you enable, the more likely it is that you will disrupt internet access for users on your network. Then you must whitelist specific domain names.

1.  Scroll down to the `EasyList` group header and click the `+` icon next to the group name. This will redirect you to the settings page to add the rule.

![Adding DNSBL category EasyList group feeds](https://www.sunnyvalley.io/docs/assets/images/Figure-42-AddingDNSBLcategoryEasyListgroupfeeds-f67c2aeb94e35351917a28a65e492966.png "Adding DNSBL category EasyList group feeds")

**Figure 42.** _Adding DNSBL category EasyList group feeds_

2.  You may set the name and description, or leave them as default.

![Setting name and description for newly added DNSBL feed](https://www.sunnyvalley.io/docs/assets/images/Figure-43-SettingnameanddescriptionfornewlyaddedDNSBLfeed-148a95a27d07b2364206bf1ee4a43a2a.png "Setting name and description for newly added DNSBL feed")

**Figure 43.** _Setting name and description for newly added DNSBL feed_

3.  You may click `Enable All` button at the bottom of the `DNSBL Source Definitions` pane to enable all feeds. But, we will enable some of the feeds such as `EasyList`, `EasyList_Adware`, `EasyList_Spanish`, `EasyList_Turkish` and `EasyPrivacy`. Select `ON` option in the `State` drop-down menu for the related feeds in the `DNSBL Source Definitions` pane. You may also select `HOLD` option if you wish to download the list once but exclude it from automatic updates.

![DNSBL source definitions for EasyList group](https://www.sunnyvalley.io/docs/assets/images/Figure-44-DNSBLsourcedefinitionsforEasyListgroup-5c0d4fcace568ed54c9bb3fe834a84b8.png "DNSBL source definitions for EasyList group")

**Figure 44.** _DNSBL source definitions for EasyList group_

4.  Scroll down to the `Settings` pane and select one of the `Action` options you wish to take when a domain name is matched.
5.  Select `Unbound` in the `Action` drop-down menu.

![DNSBL category settings to add EasyList feeds on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-45-DNSBLcategorysettingstoaddEasyListfeedsonpfBlockerNG-0c247f7a2e030ccdb159dd32e9f6a52d.png "DNSBL category settings to add EasyList feeds on pfBlockerNG")

**Figure 45.** _DNSBL category settings to add EasyList feeds on pfBlockerNG_

6.  Leave other settings as default.
7.  You may add your own domain name list that you wish to block by clicking on `+` sign icon. ![Custom DNSBL list on pfBlcokerNG](data:image/png;base64,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 "Custom DNSBL list on pfBlcokerNG")

**Figure 46.** _Custom DNSBL list on pfBlockerNG_

8.  Enter domain name to be blocked. We will add `dnsbltest.com` domain for verification of DNSBL blocking on our pfBlockerNG.
9.  Click on the `Save DNSBL Settings` button.
10.  Congratulations! You have successfully enabled DNSBL category EasyList feeds on your pfBlockerNG to protect your network.

![DNSBL Groups summary on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-47-DNSBLGroupssummaryonpfBlockerNG-01d6be25aa239fd952b49ec33e0641a3.png "DNSBL Groups summary on pfBlockerNG")

**Figure 47.** _DNSBL Groups summary on pfBlockerNG_

You can follow the similar steps given above for enabling more DNSBL groups, just add the alias group, select the lists you want to enable and choose the action to be taken when an item is matched. However, be aware that there is a memory and processing impact with each list enabled and you may overload your hardware.

#### Forcing to reload the DNSBL on pfblockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#forcing-to-reload-the-dnsbl-on-pfblockerng "Direct link to heading")

You may need to force reloading the DNSBL list. To activate the newly enabled DNSBL settings, follow these steps:

1.  Navigate to the `Firewall` -> `pfBlockerNG` -> `Update`
2.  Select `Reload` in Force option.
3.  Select `DNSBL` in Reload option.
4.  Click on `Run`.

![Forcing to reload the DNSBL list on pfblockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-48-ForcingtoreloadtheDNSBLlistonpfblockerNG-d18a8b805a55df75fc245b35bfbafc65.png "Forcing to reload the DNSBL list on pfblockerNG")

**Figure 48.** _Forcing to reload the DNSBL list on pfblockerNG_

### Verifying the DNSBL Blocking on pfBlockerNG[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#verifying-the-dnsbl-blocking-on-pfblockerng "Direct link to heading")

You may verify your DNSBL Blocking settings on pfBlockerNG by following the next steps easily.

1.  Open your favorite browser and enter the domain name that you added to the Custom DNSBL list. It is `dnsbltest.com` for our example.
2.  You should see the default blocking landing page of pfBlockerNG given below.

![DNSBL blocking landing page of pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-49-DNSBLblockinglandingpageofpfBlockerNG-dd013a3df28b5618bee65efaf450a483.png "DNSBL blocking landing page of pfBlockerNG")

**Figure 49.** _DNSBL blocking landing page of pfBlockerNG_

3.  Also, you should see the related blocks on pfBlockerNG alerts. Navigate to the `Firewal`l > `pfBlockerNG` > `Reports` -> `Alerts`.
4.  Search `dnsbltest.com` on the DNSBL Python pane.

![DNSBL alerts in pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-50-DNSBLalertsinpfBlockerNG-b957c6736136f9068d6a84d89b0473a3.png "DNSBL alerts in pfBlockerNG")

**Figure 50.** _DNSBL alerts in pfBlockerNG_

5.  Another verification method for DNSBL is viewing the DNSBL Block Stats page under Reports tab of pfBlockerNG. You may see the related blocks in `Top Blocked Domain` or `Top Blocked Evaluated Domain`, if the blocked domain is on the top blocked domain list in your firewall.

![Top Blocked Domain and Top Blocked Evaluated Domain](https://www.sunnyvalley.io/docs/assets/images/Figure-51-TopBlockedDomainandTopBlockedEvaluatedDomain-4b0217957b583d38445e4462de26d02a.png "Top Blocked Domain and Top Blocked Evaluated Domain")

**Figure 51.** _Top Blocked Domain and Top Blocked Evaluated Domain_

INFO

You may add your custom pfBlockerNG block web pages to `/usr/local/www/pfblockerng/www/` on your pfSense® software. Then activate it in the `Blocked Webpage` option of `DNSBL Configuration` pane.

6.  Lastly, you may check the result of the DNS query for `dnsbltest.com` domain in your network. Your pfSense® software DNS resolver should return the Virtual IP address(10.10.10.1 by default) of the DNSBL Web server as a result.

![nslookup for dnsbltest.com returns VIP of DNSBL server on pfBlockerNG](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYQAAACvCAMAAADKf1mgAAAAmVBMVEUMDAzMzMw0DQ3MsXZ2scwNDTTMk1c0drENV5NXk8xXDQ2xdjR2NA2TVw0NNHYNDVfMzJOTzLGxzJPMzLGTzMyxzMyxzLGTV1exsXY0V5N2NDSTzJPMk3ZXNHZ2sZN2dpN2sbF2V3axk5M0DUk0NHaxdlexsbFXk7FXV5OTsXZXDVeTVzSTk7F2k5M0dpOTk3Z2NFeTdjRXDTScXoGfAAAJyUlEQVR42uydCZOTQBCFZY2cIUaz3nd5a5X//+fZNPvtyEtBSG20JOlXxmGanqbpl4Glh5ncCwQCgUAgEAgE/m9Uzd31y/tXqfJgRU3qp0WRZflM/6b9xctW7P5VlFmWNam6eX7lxeP63r2Hj9bTbdH3jSecynwSNEoiPw7FyUnA7in8nMbOAv1rLUefQ4J+ZT6/X8tJTeJ4ErB9hiS0g28oHkGCia2ndDpVlnV7iqbwnoPc9XuvCxO7nimWr1fZs+vd2ruz7352nVmVOu2tasix27ff1shdz+p/7u9q2MMOwUI/lVVjm7u1ueeu0g7gr9iz4ueqq2MXf4iD+q9XFo5/G4dp1EKCf/0TCXy/qrz/FObC5ily9L1BR4fr2EaZtdXu5arlJCwQtOH/wTcp2W1N5Iaa3isrCquz32q7dbKHHSyiT2lmHqycS0jIWpffAH/VnuuVFkXtCcRB/QdlH3COTxxmkaDdOpGwrQm1C81wkjsIqovt4ydl/4rcPGE/QaBOe04Cu1wekCc/0n47sWRP/ED/tl31efXn8b3kOAb8VXvoKQnEQf0HVDk+cTiKBNpjhOvH5klm2K1xCTn67nTVeGUOCbTHa6T9gSDBm3jjfj8qKWjYwQL6t+0qc3qcBPwVe+MkEAf1X+8lHH8mCd5IyUwkuOGczVuXkKNvznnwvF1xmATaKwnWZKQn0IrLEfawM9YTmhJ9gu1KN8BfsTfdE9R/gP/H9gT+OuKeAIsPr2+uqZxelSsJvo2++2/X7O54ZTaHBLb8IEjtuK19TNTJOanKNmnFjRl7cpdBn9Ji5KyZ//Qg3wb4q/bQwy7+0Fb8d8+3NZfZ5MdMEgq/mycS6IrXmfnmN/n010C6ISFHv+pNNF2XuP8iHdz0vB0nSZ32XSPs8sfF7sPzK+T4wX4ChT3sYBd9SnesyNrOr+2b51ecBsBftYcedvGHHeo/JBBPjq8kzIe1PFp/IeiCHTgOQcI5IkgIBAKBfwYeKpu4Vh+FFeMJJwCPWzxINEHCPJySAB7Y/bktesKx4wk6XuBZY/tP5GPDk+T3IYH+gN29/D9yQP5e8/Cqx3gD/mAX+ULhEdkbLyAfrvIxEsjTFxZij0Y1sKv5f+TSPt/Lw6se4w34w3F7+XKe3AWlpz50vIDMosqnLidJed+u5uuR0579mn1ET9Mk+MNxl5Y+UXjWUsYLTPiq7s5d5XNJULuar0euJGgeXvUYb8AfjtvLF0xCd+3fHy+o2tI2RT4K8vSQoHY1X498Vk/QfH0pJHhPa5bdE7j2D4Nd5l/t/EU+emMm/w4Jalfz9chJAbNf8/Cqx3gD/nDcXr5YEmS8QII9JlegBglcHdih+XrkSoLm4UWP8QZIYDfypZKw7OzkhT9f/B/BCBL+g2AECYFAIHB68Ni/+Mf/ZULmFVDe4Va5rVP2VOST9eUmQe8E5hWckIRy986D6W/B5SqfrF8wCcN5BZTk6+V9fMrxJ+e3dbnlNVbLHal8qn7BJAznFVCSr0/jCQdzSMCDWW49A9uqfKJ+ySQM5xVQkq/X9/EpwSgJ9z89sUGZIGEehvMKKPnG6/v4lAdJ6No8CBJmYjCvgBIS9H18ykMkGHmt3xOChFkYziughAR5H5/y4D2hnxtACvpWPlK/eBKG8wooCbi8j085TkLVD/Tz2j5BRj5VX/LrEoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBM0T3iszyV5P8S8huljnIWSX+jusfVY3WjyZB258/ag/P5ukXfyuYdYpOSwIIEkZQ+2uQm8ffmo4E1ikCOk/By8GyxtO/J0Cd94hZx0jtoc96R9in/Zmj9tcgN49/vKqNBNYpAjpPwT9lImHi9wS07hqsY6T20Ga9I+xfTk9wEuqiMRJYpwjoPAX9vQGZv3CQBNasUHtos8rLpZLw8PtHfw9+sFqKzlPQ3xuQ+QuzSVB7aLPe0eWRsHnqwaieOQleZ6/OU9DfG5D5C7NJGNqLnvCbvXPdaRuIgrDUJllycVtValUJtY3ohR+AuLz/w2GfzWjjEZu1MIjD7nw/asUVS8gkhJzx7PTP/JX98J8u7JEZPSicU0B+AX0EuT4BgNsswng9ek9Yp/WP+wkqvnA+hjF7EWyzQN7FjHMKyC+gjyDfJ2DgNnoIkgi8HkDvAdaPX1+7CJFa+xLqRSI4QCIIIUTjLBfu3gnaeW/K+gHzR8/4XLCVCK9ZVV5m3Q1TEomQpewHFOf/AUM99BtgvyRAhRfUr8D9xqO+hLTFQPXbcp70AwrzfzxZ113qPVjlCmAM7lfgfmP0MKR9mao3fMp+QHbqmSr8w/a4Hxe+Q8K0AdyvgOX4+6R9mZqYKJ32A0rz/8/nDze3f/ap34D2S0K1L+B+BejH3yf5EKEB97PoB/ArgUX4eX95d576DeA7ZDTgfgXuNx6JELBe9SoU/IDM/B8s99fd2cWPD+g3wErcaQ+4X4H7jSHCyIcI1Y+6C35Adv6fOqwH0wj9BvZlZB8c73PE/QrcbwwRDj4E1qtdBCGEEEIIcYzyCadQPqEelE9wgPIJDlA+wQHKJzhA+QQHKJ/gAOUTHKB8wvtEn3AdIBEcIBGEEKJxlE94Dson1IPyCW+O8gkuUD7BAconOED5BAcon+AA5RMcoHyCEEIIIYR4vzwx1Tx84m3kg+2zibmEVxSBRnPrbcOXwOf4v5MIb07MJWDez37BdEcNuQWb98MX6A9Xi+E2RIDfUMg1sI+Q7ledxFwC5v3kF0wXAbkFzPsDZkv9I3z2ZcOvhEKugX0E3K9a3ZuYS8CvE/YLpq7CuYU0eOvsP1mEUq6BfYTD/ar1V1bMJWDez37B1FUot4A5f1aEUq6BfYTaRbBcgv18/EqYDucWMOfPipDPNeBcW68EyyVg3k9+wdT3BOQMkggDIYpga0EE+A2Ua+AcAvsI1YswvJli3k9+wWQRMN+HCJjzYxn4EfAbcrkGiMA+QuUiCCGEEEKI/BS0qaud/WCXbL2gCMuF/Z2PKemM8w1dRxT+7XfzRKDp52978OxqudWs8w2JsOjCNvkBOGKfI5r745j9JL38tekH15iSft/NOd+QCF839sTH9f44Yp8jnvuH0kzJ3AP7p59kdDPOtyRCvAIafgCO2OeI5/44gqwIH/9+IxNmyvlWRXhs34xRAISBIIh2Fvn/dw0Jg3AoZ9LeTLNwYGFWDOwmbaTI9AEoX3zM/dHUhP4MpcD/eVkTjg7n/dHHhJD7o5kJ3bzGP35pXtSEc7w8fQCKCSH3R7M9gTsLRNOL83ImzOaEPgBlwUPuj36bcM37BtQDLOrevJYNIiIiIiIiIiIiIvLODbMa2gzpcQAPAAAAAElFTkSuQmCC "nslookup for dnsbltest.com returns VIP of DNSBL server on pfBlockerNG")

**Figure 52.** _nslookup for dnsbltest.com returns VIP of DNSBL server on pfBlockerNG_

##### Ad-Blocking Verification[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#ad-blocking-verification "Direct link to heading")

To verify the ad-blocking feature of the pfBlockerNG, you may connect to the `yahoo.com` website on your favorite browser. You should see empty spaces in the place of advertisements on the page as given below.

![yahoo.com page with ad-blocking (ads in the red rectangles are blocked)](https://www.sunnyvalley.io/docs/assets/images/Figure-53-yahoo.compagewithad-blocking(adsintheredrectanglesareblocked)-8022dec6641d412cae960348fb9a5adc.png)

**Figure 53.** _yahoo.com page with ad-blocking (ads in the red rectangles are blocked)_

![yahoo.com page without ad-blocking](https://www.sunnyvalley.io/docs/assets/images/Figure-54-yahoo.compagewithoutad-blocking-9ba37d89f3776bc5d34e1a22fa79e5d3.png)

**Figure 54.** _yahoo.com page without ad-blocking_

## DNS over HTTPS/TLS Blocking[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#dns-over-httpstls-blocking "Direct link to heading")

pfBlockerNG allows you to block DNS over HTTPS/TLS packets on your network. It includes a comprehensive list of known public DNS servers that support DNS over HTTPS. Since DNS over HTTPS is a serious privacy and security risk, you should enable DoH/DoT(DNS over HTTPS/DNS over TLS) feature on your pfBlockerNG. Otherwise, some of your users in your network may bypass pfBlockerNG's adblocking and pfSense's DNS server.

To enable DoH/DoT Blocking you may follow the steps listed below.

1.  Navigate to the `Firewall` -> `pfBlockerNG` -> `DNSBL` -> `DNSBL SafeSearch`.
2.  Select `Enable` for DoH/DoT Blocking in the DNS over HTTPS/TLS Blocking pane
3.  Select all the DNS servers from the `DoH/DoT Blocking List` you want to block.
4.  Click `Save` button at the bottom of the page.

![Enabling DoH/DoT on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-55-EnablingDoH-DoTonpfBlockerNG-36d7d8f452593a98caaef5cfc0810e85.png "Enabling DoH/DoT on pfBlockerNG")

**Figure 55.** _Enabling DoH/DoT on pfBlockerNG_

## Enabling SafeSearch and YouTube Restrictions[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#enabling-safesearch-and-youtube-restrictions "Direct link to heading")

pfBlockerNG has a `SafeSearch` feature which will force Search sites to utilize the "Safe Search" algorithms. At the time of writing, SafeSearch is supported by Google, Yandex, DuckDuckGo, Bing and Pixabay.

pfBlockerNG also allows you to use `YouTube Restrictions` on your network. YouTube Restricted Mode filters out potentially mature videos while leaving a large number of videos still available. You may use the following settings for Youtube restrictions on your pfBlockerNG:

-   **Strict:** This setting is the most restrictive. Strict Mode does not block all videos, but works as a filter to screen out many videos based on an automated system, while leaving some videos still available for viewing.
-   **Moderate:** This setting is similar to Strict Mode but makes a much larger collection of videos available.

To enable SafeSearch and YouTube Restrictions you may follow the steps listed below.

1.  Navigate to the `Firewall` -> `pfBlockerNG` -> `DNSBL` -> `DNSBL SafeSearch`.
2.  Select `Enable` for SafeSearch Redirection in the SafeSearch settings pane.
3.  You may select `Moderate` or `Strict` to enable YouTube Restrictions.
4.  Click the `Save` button at the bottom of the page.

![SafeSearch settings on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-56-SafeSearchsettingsonpfBlockerNG-4576b8f5f68e588edf35c0bb95baa11c.png "SafeSearch settings on pfBlockerNG")

**Figure 56.** _SafeSearch settings on pfBlockerNG_

## Whitelisting[​](https://www.sunnyvalley.io/docs/network-security-tutorials/pfblockerng#whitelisting "Direct link to heading")

While you shouldn't have too many problems as long as you don't get too innovative with your blocklists, rightful services may be blocked in some cases. This may be a result of genuine false positives, but it can also be an indication that a legitimate site has been hacked and is now sending malicious traffic, so always be careful before whitelisting. Because the blocklists are frequently updated, these issues are often temporary.

When you need to whitelist something on pfBlockerNG, you can follow the next steps below:

1.  Navigate to `Firewall` -> `pfBlockerNG` -> `Reports` -> `Alerts`.
    
2.  Look through the list of recent blocks and add the offending item to the whitelist by clicking the `+` icon next to it. For example, we will add the `dnsbltest.com` domain that we use for DNSBL testing to the whitelist. This will pop up a confirmation message.
    

![Domain Whitelisting on pfBlockerNG](https://www.sunnyvalley.io/docs/assets/images/Figure-57-DomainWhitelistingonpfBlockerNG-e901737e1301516c1e075cb6e7bd0073.png)

**Figure 57.** _Domain Whitelisting on pfBlockerNG_

3.  Click `OK`.
    
4.  It will ask you if you want to whitelist this domain only or add a wildcard for the domain. Select as you wish.
    

![Domain Whitelisting on pfBlockerNG-2](data:image/png;base64,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 "Domain Whitelisting on pfBlockerNG-2")

**Figure 58.** _Domain Whitelisting on pfBlockerNG-2_

5.  Then, you will have the option to add a description. To enter a description click on `Yes` and then enter a description.

![Enter description for whitelist](data:image/png;base64,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 "Enter description for whitelist")

**Figure 59.** _Enter a description for whitelist_

6.  The `pfBlockerNG` will no longer block the whitelisted domain.

![Whitelisting completed successfully](data:image/png;base64,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)

**Figure 60.** _Whitelisting completed successfully_