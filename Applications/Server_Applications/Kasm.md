---
title: Kasm
description: 
dateCreated: 2022-05-26T18:23:12.715Z
published: true
editor: markdown
tags:
  - Server
  - Docker
dateModified: 
---
# KASM

KASM is an environment where you can run isolated browsers and Operating systems

	- https://www.kasmweb.com/docs/latest/install/single_server_install.html

	![Capture](https://user-images.githubusercontent.com/12887622/134812745-f25c087d-f3ca-4707-ae6a-cd74859bcf8a.JPG)

I run Kasm on a Truenas Scale server. 

## Troubleshooting
### Admin Account Recovery(https://www.kasmweb.com/docs/latest/how_to/admin_account_recovery.html#admin-account-recovery "Permalink to this headline")

Admins may find themselves locked out of their accounts by forgetting or mis-setting their account passwords. This guide will show show the steps for resetting the admin account password.

1.  SSH to the Kasm Workspaces server and connect to the database with the following command:
    
    > sudo docker exec -it kasm_db psql -U kasmapp -d kasm
    
2.  Reset the Admin password and exit the psql shell
    
    > update users set
    >     pw_hash = 'fe519184b60a4ef9b93664a831502578499554338fd4500926996ca78fc7f522',
    >     salt = '83d0947a-bf55-4bec-893b-63aed487a05e',
    >     secret=NULL, set_two_factor=False, locked=False,
    >     disabled=False, failed_pw_attempts = 0 where username ='admin@kasm.local';
    > \q
    
3.  Login to the Workspaces UI using “admin@kasm.local” with the password “password” and reset your password to a secure password.
    

[![../_images/password_change.jpg](https://www.kasmweb.com/docs/latest/_images/password_change.jpg)](https://www.kasmweb.com/docs/latest/_images/password_change.jpg)

**Reset Password Location**[¶](https://www.kasmweb.com/docs/latest/how_to/admin_account_recovery.html#id1 "Permalink to this image")

# Login SSO with Authentik

## Authentik Side

Login as Admin

goto Application -> Providers -> Create

Click OAuth2/OpenID Provider -> next

Select your authentication Flow

Select your Authorization Flow

Under Protocol Settings select Confidential

Copy your ClienID (you will paste it into KASM)

Copy your Secret (you will paste it into kasm)

Redirect URIs/Origins (copy from your kasm SSO setup (see final box in "Kasm Side")

## Kasm Side

Below example has authentik.commsnet.org as the application URL this should be replaced with your authentik instance URL

ie `authentik.example.com` or `authentik.mydomain.tld`

Display Name

`login with authentik` or whatever you want displayed

Logo URL

`Blank` or find a nice logo

Enabled

`toggle set to on`

Auto Login

`toggle set to off`

Hostname

`Blank`

Default

`toggle set to on`

Client ID

`copy from authentik provider`

Client Secret

`Copy from authentik provider`

Authorization URL

`https://authentik.commsnet.org/application/o/authorize/`

Token URL

`https://authentik.commsnet.org/application/o/token/`

User Info URL

`https://authentik.commsnet.org/application/o/token/`

Scope (One Per Line)

```
openid
email
profile
```

Username Attribute

`email`

Groups Attribute

`groups`

Debug

`toggle set to off`

Redirect URL

*Copy the Redirect URL to your provider in authentik*

## Mapping SSO Groups in Kasm

After the SSO is setup click groups in Kasm

edit a group

click the SSO Group Mappings tab

click add SSO Mapping

Select your SSO Provider in the dropdown

uncheck "Assign All Users"

Add your group name `service_admins` or `admins` for the users group `Users` this should be exactly like it is in your authentik instance. (in authentik as admin click directory then groups )