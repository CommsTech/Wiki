---
title: Simple Mail Transport Protocol (SMTP)
description: Simple Mail Transfer Protocol (SMTP) is the standard protocol used for sending and receiving emails over the internet. It defines how email messages are formatted, addressed, and transmitted between mail servers.
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Simple Mail Transfer Protocol (SMTP)

**Simple Mail Transfer Protocol (SMTP)** is the standard protocol used for sending and receiving emails over the internet. It defines how email messages are formatted, addressed, and transmitted between mail servers.

## History

SMTP was first published in 1982 as an extension of the original email system called "Mail Transfer Protocol" (MTP). The current version, SMTPv4, was published in 1985.

## How it works
1. A client (email client or mail user agent) sends a request to a mail transfer agent (MTA) on the sender's mail server.
2. The MTA checks if it can send the email to the recipient's domain's mail server. If not, it forwards the email to another MTA until it reaches the recipient's mail server.
3. The recipient's mail server receives the email and stores it in the recipient's mailbox or rejects it if necessary.

## Features
1. Text-based protocol that uses plain text commands and responses.
2. Supports both synchronous and asynchronous communication between servers.
3. Provides error messages for failed delivery attempts.
4. Allows email filtering based on various criteria, such as sender or recipient addresses.

## Security concerns
1. SMTP does not provide end-to-end encryption, meaning the content of emails can be intercepted and read by third parties during transmission.
2. It is susceptible to email spoofing, where an attacker sends emails with a fake sender address.
3. Open relays can be used to send spam or phishing emails.

## Authentication methods
1. **Username/Password authentication** - The client authenticates itself using a username and password.
2. **TLS (Transport Layer Security)** - Encrypts the communication between the client and server, protecting the content of emails from being intercepted.
3. **DKIM (DomainKeys Identified Mail)** - Verifies that an email was sent from an authorized mail server.
4. **SPF (Sender Policy Framework)** - Allows a domain owner to specify which servers are allowed to send emails on their behalf.

## Troubleshooting

### [[Transport_Layer_Security_tls|Transport Layer Security (TLS)]]

1. Prepare encoded strings for your mail username and password

```bash
echo -ne "mail@example.net" | base64
```

2. Connect to mail server

```bash
openssl s_client -starttls smtp -connect smtp.example.com:587
```

3. Send HELO

```smtp
EHLO
```

4. Authenticate

```smtp
AUTH LOGIN
<your-encoded-username>
<your-encoded-password>
```

If that's successful the mail server should return `235 2.7.0 Authentication successful`.
