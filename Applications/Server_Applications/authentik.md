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

## What is authentik?

authentik is an open-source Identity Provider that emphasizes flexibility and versatility. It can be seamlessly integrated into existing environments to support new protocols. authentik is also a great solution for implementing sign-up, recovery, and other similar features in your application, saving you the hassle of dealing with them.

## Installation

For small/test setups it is recommended to use Docker Compose; refer to the [documentation](https://goauthentik.io/docs/installation/docker-compose/?utm_source=github).

For bigger setups, there is a Helm Chart [here](https://github.com/goauthentik/helm). This is documented [here](https://goauthentik.io/docs/installation/kubernetes/?utm_source=github).

## Screenshots

| Light                                                  | Dark                                                  |
| ------------------------------------------------------ | ----------------------------------------------------- |
| ![](https://goauthentik.io/img/screen_apps_light.jpg)  | ![](https://goauthentik.io/img/screen_apps_dark.jpg)  |
| ![](https://goauthentik.io/img/screen_admin_light.jpg) | ![](https://goauthentik.io/img/screen_admin_dark.jpg) |

## Development

See [Developer Documentation](https://goauthentik.io/developer-docs/?utm_source=github)

## Security

See [SECURITY.md](SECURITY.md)

## Adoption and Contributions

Your organization uses authentik? We'd love to add your logo to the readme and our website! Email us @ hello@goauthentik.io or open a GitHub Issue/PR! For more information on how to contribute to authentik, please refer to our [CONTRIBUTING.md file](./CONTRIBUTING.md).

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