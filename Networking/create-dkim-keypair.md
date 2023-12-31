---
title: Create DKIM keypair
description: 
dateCreated: 2022-05-21T15:28:21.146Z
published: true
editor: markdown
tags: 
dateModified: 
---
# Generate a DKIM Key pair
We use the tool OpenSSL ([[openssl]]]) to generate a DKIM private and public key pair.

`openssl genrsa -out dkim_private.pem 2048`

`openssl rsa -in dkim_private.pem -pubout -outform der 2>/dev/null | openssl base64 -A`

