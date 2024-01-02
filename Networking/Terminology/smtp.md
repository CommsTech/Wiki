---
title: Simple Mail Transport Protocol (SMTP)
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Simple Mail Transport Protocol




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
