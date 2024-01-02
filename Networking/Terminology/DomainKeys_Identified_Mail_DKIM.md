---
title: DomainKeys Identified Mail (DKIM)
description: DomainKeys Identified Mail (DKIM) is an email authentication method designed to prevent email spoofing and ensure the authenticity of incoming emails. It adds an extra layer of security by verifying that the email was sent from an authorized mail server, thus reducing the risk of phishing attacks and email-based spam.
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# DomainKeys Identified Mail (DKIM)

DomainKeys Identified Mail (DKIM) is an email authentication method designed to prevent email spoofing and ensure the authenticity of incoming emails. It adds an extra layer of security by verifying that the email was sent from an authorized mail server, thus reducing the risk of phishing attacks and email-based spam.

How it works:

1. The sender's email domain sets up a DKIM record in their Domain Name System (DNS) that includes the public key used to sign emails.
2. When an email is sent, the mail server signs the email using its private key.
3. The receiving mail server checks the DKIM record and verifies the signature on the email using the sender's public key.
4. If the verification is successful, the email is considered authentic and delivered to the recipient's inbox.

Benefits:

1. Enhances email security by preventing spoofing and ensuring email authenticity.
2. Improves deliverability of emails by increasing trust between mail servers.
3. Reduces the risk of phishing attacks and spam.

Configuration:  
To set up DKIM for your domain, follow these steps:

1. Generate a public-private key pair.
2. Add the DNS records (TXT and CNAME) to your domain's DNS settings.
3. Configure your email server to sign emails using the private key.
4. Verify the DKIM signature on incoming emails.

Tools:

1. Mail-Tail - A free, open-source tool for managing and monitoring DKIM keys.
2. MXToolbox - An online tool that can help you check your DKIM records and verify email authentication.
3. Google Postmaster Tools - Allows you to manage and monitor your domain's email sending reputation and performance.

Additional Resources:

1. [DKIM Record Lookup](https://mxtoolbox.com/dkim.aspx)
2. [Setting up DKIM with Google Workspace](https://support.google.com/a/answer/33678)
3. [DKIM Best Practices](https://www.dkim.org/faq/)