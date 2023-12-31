---
title: Authentik
description: 
dateCreated: 
published: 
editor: markdown
tags:
  - Authentication
  - LDAP
  - SAML
  - OIDC
dateModified: 
---
# Authentik

## Accidentally deleted the akadmin
from the cli
```
ak shell
```

then

```
User.objects.create(username="akadmin")
```

then you can follow [https://goauthentik.io/docs/troubleshooting/missing_admin_group](https://goauthentik.io/docs/troubleshooting/missing_admin_group) to make that user admin and the recovery key command above to login as that user

## Accidentally disabled the akadmin account

for this example we will create another admin with the username helpme.
from the cli
```
ak createsuperuser helpme
ak changepassword helpme
```
enter the password twice
then verify it exists
```
ak shell
User.objects.create(username="helpme")
```
then add to the akadmins group
```
ak create_admin_group helpme
```