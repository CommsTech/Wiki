---
title: Authelia
description: how to configure LDAP and Authelia on truenas scale
dateCreated: 2022-07-18T02:41:39.777Z
published: true
editor: markdown
tags:
  - LDAP
dateModified: 
---
# Authelia

**Authelia** is an open-source authentication and authorization server providing two-factor authentication and single

sign-on (SSO) for your applications via a web portal. It acts as a companion for [reverse proxies](#proxy-support) by

allowing, denying, or redirecting requests.

Documentation is available at [https://www.authelia.com/](https://www.authelia.com/).

The following is a simple diagram of the architecture:

<p align="center" style="margin:50px">
  <img src="https://www.authelia.com/images/archi.png"/>
</p>

**Authelia** can be installed as a standalone service from the [AUR](https://aur.archlinux.org/packages/authelia/),

[APT](https://apt.authelia.com/stable/debian/packages/authelia/),

[FreeBSD Ports](https://svnweb.freebsd.org/ports/head/www/authelia/), or using a

[static binary](https://github.com/authelia/authelia/releases/latest),

[.deb package]((https://github.com/authelia/authelia/releases/latest)), as a container on [Docker] or [Kubernetes].

Deployment can be orchestrated via the Helm [Chart](https://charts.authelia.com) (beta) leveraging ingress controllers

and ingress configurations.

<p align="center">
  <img src="https://www.authelia.com/images/logos/kubernetes.png" height="100"/>
  <img src="https://www.authelia.com/images/logos/docker.logo.png" width="100">
</p>

Here is what Authelia's portal looks like:

<p align="center">
  <img src="https://www.authelia.com/images/1FA.png" width="400" />
  <img src="https://www.authelia.com/images/2FA-METHODS.png" width="400" />
</p>

## Features summary

This is a list of the key features of Authelia:

* Several second factor methods:

  * **[Security Keys](https://www.authelia.com/overview/authentication/security-key/)** that support

    [FIDO2]&nbsp;[WebAuthn] with devices like a [YubiKey].

  * **[Time-based One-Time password](https://www.authelia.com/overview/authentication/one-time-password/)**

    with compatible authenticator applications.

  * **[Mobile Push Notifications](https://www.authelia.com/overview/authentication/push-notification/)**

    with [Duo](https://duo.com/).

* Password reset with identity verification using email confirmation.

* Access restriction after too many invalid authentication attempts.

* Fine-grained access control using rules which match criteria like subdomain, user, user group membership, request uri,

 request method, and network.

* Choice between one-factor and two-factor policies per-rule.

* Support of basic authentication for endpoints protected by the one-factor policy.

* Highly available using a remote database and Redis as a highly available KV store.

* Compatible with [Traefik](https://doc.traefik.io/traefik) out of the box using the

  [ForwardAuth](https://doc.traefik.io/traefik/middlewares/http/forwardauth/) middleware.

* Curated configuration from [LinuxServer](https://www.linuxserver.io/) via their

  [Swag](https://docs.linuxserver.io/general/swag) container as well as a

  [guide](https://blog.linuxserver.io/2020/08/26/setting-up-authelia/).

* Compatible with [Caddy] using the [forward_auth](https://caddyserver.com/docs/caddyfile/directives/forward_auth)

  directive.

* Kubernetes Support:

  * Compatible with several Kubernetes ingress controllers:

    * [ingress-nginx](https://www.authelia.com/integration/kubernetes/nginx-ingress/)

    * [Traefik Kubernetes CRD](https://www.authelia.com/integration/kubernetes/traefik-ingress/#ingressroute)

    * [Traefik Kubernetes Ingress](https://www.authelia.com/integration/kubernetes/traefik-ingress/#ingress)

    * [Istio](https://www.authelia.com/integration/kubernetes/istio/)

  * Beta support for installing via Helm using our [Charts](https://charts.authelia.com).

* Beta support for [OpenID Connect](https://www.authelia.com/roadmap/active/openid-connect/).

For more details take a look at the [Overview](https://www.authelia.com/overview/prologue/introduction/).

If you want to know more about the roadmap, follow [Roadmap](https://www.authelia.com/roadmap).

## Proxy support

Authelia works in combination with [nginx], [Traefik], [Caddy], [Skipper], [Envoy], or [HAProxy].

<p align="center">
  <img src="https://www.authelia.com/images/logos/nginx.png" height="50"/>
  <img src="https://www.authelia.com/images/logos/traefik.png" height="50"/>
  <img src="https://www.authelia.com/images/logos/caddy.png" height="50"/>
  <img src="https://www.authelia.com/images/logos/envoy.png" height="50"/>
  <img src="https://www.authelia.com/images/logos/haproxy.png" height="50"/>
</p>

## Getting Started

See the [Get Started Guide](https://www.authelia.com/integration/prologue/get-started/) or one of the curated examples

below.

### docker-compose

The `docker-compose` bundles act as a starting point for anyone wanting to see Authelia in action. You will have to

customize them to your needs as they come with self-signed certificates.

#### [Local](https://www.authelia.com/integration/deployment/docker/#local)

The Local compose bundle is intended to test Authelia without worrying about configuration.

It's meant to be used for scenarios where the server is not be exposed to the internet.

Domains will be defined in the local hosts file and self-signed certificates will be utilised.

#### [Lite](https://www.authelia.com/integration/deployment/docker/#lite)

The Lite compose bundle is intended for scenarios where the server will be exposed to the internet, domains and DNS will

need to be setup accordingly and certificates will be generated through LetsEncrypt. The Lite element refers to minimal

external dependencies; File based user storage, SQLite based configuration storage. In this configuration, the service

will not scale well.

## Deployment

Now that you have tested **Authelia** and you want to try it out in your own infrastructure,

you can learn how to deploy and use it with [Deployment](https://www.authelia.com/docs/deployment/deployment-ha).

This guide will show you how to deploy it on bare metal as well as on

[Kubernetes](https://kubernetes.io/).

## Security

Authelia takes security very seriously. If you discover a vulnerability in Authelia, please see our

[Security Policy](https://github.com/authelia/authelia/security/policy).

For more information about [security](https://www.authelia.com/information/security/) related matters, please read

[the documentation](https://www.authelia.com/information/security/).

## Contact Options

Several contact options exist for our community, the primary one being [Matrix](#matrix). These are in addition to

[GitHub issues](https://github.com/authelia/authelia/issues) for creating a

[new issue](https://github.com/authelia/authelia/issues/new/choose).

### Matrix

Community members are invited to join the [Matrix Space](https://matrix.to/#/#community:authelia.com) which includes

both the [Support Room](https://matrix.to/#/#support:authelia.com) and the

[Contributing Room](https://matrix.to/#/#contributing:authelia.com).

- The core team members are identified as administrators in the Space and individual Rooms.
- All channels are linked to [Discord](#discord).

### Discord

Community members are invited to join the [Discord Server](https://discord.authelia.com).

- The core team members are identified by the <span style="color:#BA55D3;">**CORE TEAM**</span> role in Discord.
- The [#support] and [#contributing] channels are linked to [Matrix](#matrix).

### Email

You can contact the core team by email via [team@authelia.com](mailto:team@authelia.com). Please note the

[security@authelia.com](mailto:security@authelia.com) is also available but is strictly reserved for [security] related

matters.

## Breaking changes

Since Authelia is still under active development, it is subject to breaking changes. It's recommended to pin a version

tag instead of using the `latest` tag and reading the [release notes](https://github.com/authelia/authelia/releases)

before upgrading. This is where you will find information about breaking changes and what you should do to overcome

said changes.

## Why Open Source?

You might wonder why Authelia is open source while it adds a great deal of security and user experience to your

infrastructure at zero cost. It is open source because we firmly believe that security should be available for all to

benefit in the face of the battlefield which is the Internet, with near zero effort.

Additionally, keeping the code open source is a way to leave it auditable by anyone who is willing to contribute. This

way, you can be confident that the product remains secure and does not act maliciously.

It's important to keep in mind Authelia is not directly exposed on the

Internet (your reverse proxies are) however, it's still the control plane for your internal security so take care of it!

## Contribute

If you want to contribute to Authelia, please read our [contribution guidelines](CONTRIBUTING.md).

Authelia exists thanks to all the people who contribute so don't be shy, come chat with us on either [Matrix](#matrix)

or [Discord](#discord) and start contributing too.

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

## Authelia and LDAP integration

https://www.youtube.com/watch?v=cmMm5keX1vk