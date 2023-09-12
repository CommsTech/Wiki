
--boundary_.oOo._ZCvtQORbT684jm1zwnaZBE7ChmgJ7q3/
Content-Length: 14709
Content-Type: application/octet-stream
X-File-MD5: 0001a07ae936bae99626088bf99f0867
X-File-Mtime: 1678632851
X-File-Path: /Wiki/Red_Team/Spoofing_365.md

# Spoofing Microsoft 365 Like its 1995
https://www.blackhillsinfosec.com/spoofing-microsoft-365-like-its-1995/


[Steve Borosh](https://twitter.com/424f424f) //

![](https://www.blackhillsinfosec.com/wp-content/uploads/2022/05/BLOG_chalkboard_00593-1024x576.jpg)

## Why Phishing?

Those of us on the offensive side of security often find ourselves in the position to test our clients’ resilience to phishing attacks. According to the Verizon 2021 Data Breach Investigations Report,[1](https://www.blackhillsinfosec.com/spoofing-microsoft-365-like-its-1995/#footnote) phishing comprises 25% of all breaches. Phishing remains one of the top ways adversaries enter networks.

### Defense-in-Depth

The phrase “defense-in-depth” has been used in the information security realm for a few years now, meaning defenders layer their protections instead of leaning too hard on a single solution. Email security especially requires the use of a defense-in-depth approach to phishing attacks. Enterprises may monitor for newly created phishing domains, filter, or block spam in the cloud, set SPF, DKIM, DMARC protection records, and scan or block file attachments. Along with user awareness training and offensive training engagements, these protections provide multiple layers of defense that create hurdles adversaries must clear, reducing their chances for success.

### Phishing Engagements

There are several types of phishing engagements often used for testing enterprises. Some types are:

-   Click-rate tracking
    -   Who clicked?
    -   How many times?
-   Credential harvesting
    -   Passwords
    -   Cookie theft
-   Payload (attached or linked)
    -   Malicious Office Document (MalDoc)
    -   Executables
    -   Compressed files

Many organizations have automated phishing training. Often, these programs require users to click a link in an email which tracks their “bad” behavior. These training scenarios are great to introduce users to the potential hazards of phishing attacks, however, they may miss the mark when modeling more advanced adversaries.

### Offensive Perspectives

Phishing is the bane of many when trying to gain access to their target enterprise. Phishing takes time and patience — lots of patience. If you’re on offense with a limited timeframe and budget, getting through all the defensive layers, and staying there, takes precision. Even with all the patience and care taken to set up infrastructure, craft a phishing email, create a payload, and get it through defenses, all it takes is one user to report the phish and it’s back to square one. Setting up new infrastructure, creating new pretext, generating new payloads, and sending from a new source all without being detected takes time, and again, patience.

What if we could cut out the infrastructure pieces, skip past domain categorization, reputation, and “bypass” all the target enterprises’ defenses with one command? Sound difficult? Let’s dive in.

## Microsoft Direct Send

Microsoft has documentation on a feature named “direct send”.[2](https://www.blackhillsinfosec.com/spoofing-microsoft-365-like-its-1995/#footnote) Direct send is most used by devices such as printers that print or scan to email inside an enterprise. Direct scan requires no authentication and may be sent from **_outside_** of the enterprise network.

Prerequisites: Microsoft 365 subscription and an [Exchange Online Plan](https://products.office.com/exchange/compare-microsoft-exchange-online-plans).

Direct send connects to an MX endpoint called a “smart host.” The smart host is in the format of “company-com.mail.protection.outlook.com” and is created by default when a new Exchange Online plan is created. Your device or host connects to the smart host via telnet on port 25 and sends unauthenticated email to internal users. Outbound emails are blocked. See the mail flow in the diagram[3](https://www.blackhillsinfosec.com/spoofing-microsoft-365-like-its-1995/#footnote) below.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2022/05/Picture1-1.png)

Figure 1 – Direct Send Mail Flow

Settings for direct send:

-   MX endpoint, company-com.mail.protection.outlook.com
-   Port 25 (yes, 25)
-   TLS/StartTLS (optional)
-   Email address (does not need to have a mailbox)
-   Recommended SPF settings from Microsoft
    -   “v=spf1 ip4:<Static IP Address> include:spf.protection.outlook.com ~all”

## Spoofing

With Microsoft direct send, inbound email will make it into the enterprise if that domain is trusted. So, in most cases with direct send, we can send mail from hr@company.com to anyone else inside company.com since the domain will trust itself. In many cases, we’re also able to spoof external email addresses to internal users if those domains are trusted by the mail gateway — such as, “account-security-noreply@accountprotection.microsoft.com” (used from Microsoft security emails) could be used as a From address.

Microsoft direct send does not allow mail to be delivered outside of the enterprise. So, no spoofing internal to external.

An added benefit of spoofing is that the From field populates with the From user’s Microsoft icon as well. If we send an email from noreply@bigtimebank.com to user@company.com, the email from field will show the avatar of the “noreply” user, typically the company logo.

To test this against your own newly created Exchange Online plan, add a “Bypass Spam Filter” rule in the exchange admin center.[4](https://www.blackhillsinfosec.com/spoofing-microsoft-365-like-its-1995/#footnote)

![](https://www.blackhillsinfosec.com/wp-content/uploads/2022/05/Picture2-1.png)

Figure 2 – Bypass Spam Filters for Trusted Domains

This rule allows internal emails to land in the inbox instead of “Junk” on default initial installations.

### Testing a Spoof

Sending a spoofed email is as simple as using a PowerShell command.

_Here’s an example PowerShell command:_

```
Send-MailMessage -SmtpServer company-com.mail.protection.outlook.com -To frank@company.com -From informationsecurity@company.com -Subject “Urgent Update Required” -Body “Frank,<br>We need you to update your Microsoft Office software. Run this update as soon as you can please. No need to let me know when it’s complete. <a href=’https://myphishsite.azurewebsites.net/’>Download</a>” -BodyAsHTML
```

This PowerShell cmdlet can also be found in PowerShell for Linux. With that in mind, it’s possible to send your phishing emails from just about anywhere. One of my favorites is to send directly from Azure Cloud Shell, which is easily accessible from Windows Terminal. It’s easy to rotate IP addresses this way. Azure Cloud Shell is easily accessible from the Windows Terminal App.

![](https://www.blackhillsinfosec.com/wp-content/uploads/2022/05/Picture3-1.png)

Figure 3 – Windows Terminal App

![](https://www.blackhillsinfosec.com/wp-content/uploads/2022/05/Picture4-1.png)

Figure 4 – Sending an Email from Azure Cloud Shell

#### SPAMHAUS

SPAMHAUS will block most residential IP addresses from sending emails. Don’t send phishing emails from your house.

#### MAIL GATEWAYS

In testing against enterprises that utilize third-party mail gateways, spoofing has been extremely successful using this technique. While mail may still flow through the email gateway, default configurations may trust email originating from their own domain and *.mail.protection.outlook.com.

#### EXCHANGE ONLINE PROTECTION

Exchange Online Protection (EOP) is a Microsoft cloud-based email filter that protects enterprises against email threats. EOP is included by default with all Microsoft 365 enterprises using Exchange Online mailboxes. Keep in mind that “Microsoft Defender for Office 365” is a separate offering and not covered in this blog post.

Email flows through Exchange Online as detailed in the diagram[5](https://www.blackhillsinfosec.com/s