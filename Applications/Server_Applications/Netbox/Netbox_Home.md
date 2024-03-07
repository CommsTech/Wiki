---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Netbox_Home

[![NetBox logo](https://raw.githubusercontent.com/netbox-community/netbox/develop/docs/netbox_logo.svg)](https://raw.githubusercontent.com/netbox-community/netbox/develop/docs/netbox_logo.svg)

**The cornerstone of every automated network**

[![Latest release](https://camo.githubusercontent.com/7527b47bcc9bbe8e5e4bc322aa7d4db4e656bec6be1bc2ece439661b0e61697a/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f762f72656c656173652f6e6574626f782d636f6d6d756e6974792f6e6574626f78)](https://github.com/netbox-community/netbox/releases) [![License](https://camo.githubusercontent.com/60a41e393583e46cafea156109ad319964506b6d6f457354dbbfbcb314b337bf/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4170616368655f322e302d626c75652e737667)](https://github.com/netbox-community/netbox/blob/master/LICENSE.txt) [![Contributors](https://camo.githubusercontent.com/767bc124ba3432234e4a3a88d25e9e210d592ba68417105518a52ff83b20d5ba/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f636f6e7472696275746f72732f6e6574626f782d636f6d6d756e6974792f6e6574626f783f636f6c6f723d626c7565)](https://github.com/netbox-community/netbox/graphs/contributors) [![GitHub stars](https://camo.githubusercontent.com/c9518f8f0b88df9681b2a068889917a415a03cd23e39922b2badb7bd98d2c024/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f73746172732f6e6574626f782d636f6d6d756e6974792f6e6574626f783f7374796c653d666c6174)](https://github.com/netbox-community/netbox/stargazers) [![Languages supported](https://camo.githubusercontent.com/958429ea78d955a1434e3541c17b726448b172f6bbc31e4484e81ce6d27645f8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c616e6775616765732d372d626c7565)](https://explore.transifex.com/netbox-community/netbox/) [![CI status](https://github.com/netbox-community/netbox/workflows/CI/badge.svg?branch=master)](https://github.com/netbox-community/netbox/actions/workflows/ci.yml)

NetBox exists to empower network engineers. Since its release in 2016, it has become the go-to solution for modeling and documenting network infrastructure for thousands of organizations worldwide. As a successor to legacy IPAM and DCIM applications, NetBox provides a cohesive, extensive, and accessible data model for all things networked. By providing a single robust user interface and programmable APIs for everything from cable maps to device configurations, NetBox serves as the central source of truth for the modern network.

[NetBox's Role](https://github.com/netbox-community/netbox#netboxs-role) | [Why NetBox?](https://github.com/netbox-community/netbox#why-netbox) | [Getting Started](https://github.com/netbox-community/netbox#getting-started) | [Get Involved](https://github.com/netbox-community/netbox#get-involved) | [Project Stats](https://github.com/netbox-community/netbox#project-stats) | [Screenshots](https://github.com/netbox-community/netbox#screenshots)

[![NetBox user interface screenshot](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/home-light.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/home-light.png)

## NetBox's Role

[](https://github.com/netbox-community/netbox#netboxs-role)

NetBox functions as the **source of truth** for your network infrastructure. Its job is to define and validate the _intended state_ of all network components and resources. NetBox does not interact with network nodes directly; rather, it makes this data available programmatically to purpose-built automation, monitoring, and assurance tools. This separation of duties enables the construction of a robust yet flexible automation system.

[![Reference network automation architecture](https://github.com/netbox-community/netbox/raw/develop/docs/media/misc/reference_architecture.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/misc/reference_architecture.png)

The diagram above illustrates the recommended deployment architecture for an automated network, leveraging NetBox as the central authority for network state. This approach allows your team to swap out individual tools to meet changing needs while retaining a predictable, modular workflow.

## Why NetBox?

[](https://github.com/netbox-community/netbox#why-netbox)

### Comprehensive Data Model

[](https://github.com/netbox-community/netbox#comprehensive-data-model)

Racks, devices, cables, IP addresses, VLANs, circuits, power, VPNs, and lots more: NetBox is built for networks. Its comprehensive and thoroughly inter-linked data model provides for natural and highly structured modeling of myriad network primitives that just isn't possible using general-purpose tools. And there's no need to waste time contemplating how to build out a database: Everything is ready to go upon installation.

### Focused Development

[](https://github.com/netbox-community/netbox#focused-development)

NetBox strives to meet a singular goal: Provide the best available solution for making network infrastructure programmatically accessible. Unlike "all-in-one" tools which awkwardly bolt on half-baked features in an attempt to check every box, NetBox is committed to its core function. NetBox provides the best possible solution for modeling network infrastructure, and provides rich APIs for integrating with tools that excel in other areas of network automation.

### Extensible and Customizable

[](https://github.com/netbox-community/netbox#extensible-and-customizable)

No two networks are exactly the same. Users are empowered to extend NetBox's native data model with custom fields and tags to best suit their unique needs. You can even write your own plugins to introduce entirely new objects and functionality!

### Flexible Permissions

[](https://github.com/netbox-community/netbox#flexible-permissions)

NetBox includes a fully customizable permission system, which affords administrators incredible granularity when assigning roles to users and groups. Want to restrict certain users to working only with cabling and not be able to change IP addresses? Or maybe each team should have access only to a particular tenant? NetBox enables you to craft roles as you see fit.

### Custom Validation & Protection Rules

[](https://github.com/netbox-community/netbox#custom-validation--protection-rules)

The data you put into NetBox is crucial to network operations. In addition to its robust native validation rules, NetBox provides mechanisms for administrators to define their own custom validation rules for objects. Custom validation can be used both to ensure new or modified objects adhere to a set of rules, and to prevent the deletion of objects which don't meet certain criteria. (For example, you might want to prevent the deletion of a device with an "active" status.)

### Device Configuration Rendering

[](https://github.com/netbox-community/netbox#device-configuration-rendering)

NetBox can render user-created Jinja2 templates to generate device configurations from its own data. Configuration templates can be uploaded individually or pulled automatically from an external source, such as a git repository. Rendered configurations can be retrieved via the REST API for application directly to network devices via a provisioning tool such as Ansible or Salt.

### Custom Scripts

[](https://github.com/netbox-community/netbox#custom-scripts)

Complex workflows, such as provisioning a new branch office, can be tedious to carry out via the user interface. NetBox allows you to write and upload custom scripts that can be run directly from the UI. Scripts prompt users for input and then automate the necessary tasks to greatly simplify otherwise burdensome processes.

### Automated Events

[](https://github.com/netbox-community/netbox#automated-events)

Users can define event rules to automatically trigger a custom script or outbound webhook in response to a NetBox event. For example, you might want to automatically update a network monitoring service whenever a new device is added to NetBox, or update a DHCP server when an IP range is allocated.

### Comprehensive Change Logging

[](https://github.com/netbox-community/netbox#comprehensive-change-logging)

NetBox automatically logs the creation, modification, and deletion of all managed objects, providing a thorough change history. Changes can be attributed to the executing user, and related changes are grouped automatically by request ID.

Note

A complete list of NetBox's myriad features can be found in [the introductory documentation](https://docs.netbox.dev/en/stable/introduction/).

## Getting Started

[](https://github.com/netbox-community/netbox#getting-started)

- Just want to explore? Check out [our public demo](https://demo.netbox.dev/) right now!
- The [official documentation](https://docs.netbox.dev/) offers a comprehensive introduction.
- Check out [our wiki](https://github.com/netbox-community/netbox/wiki/Community-Contributions) for even more projects to get the most out of NetBox!

[![NetBox Cloud](https://github.com/netbox-community/netbox/raw/develop/docs/media/misc/netbox_cloud.png)](https://netboxlabs.com/netbox-cloud/)  
Looking for an enterprise solution? Check out **[NetBox Cloud](https://netboxlabs.com/netbox-cloud/)**!

## Get Involved

[](https://github.com/netbox-community/netbox#get-involved)

- Follow [@NetBoxOfficial](https://twitter.com/NetBoxOfficial) on Twitter!
- Join the conversation on [the discussion forum](https://github.com/netbox-community/netbox/discussions) and [Slack](https://netdev.chat/)!
- Already a power user? You can [suggest a feature](https://github.com/netbox-community/netbox/issues/new?assignees=&labels=type%3A+feature&template=feature_request.yaml) or [report a bug](https://github.com/netbox-community/netbox/issues/new?assignees=&labels=type%3A+bug&template=bug_report.yaml) on GitHub.
- Contributions from the community are encouraged and appreciated! Check out our [contributing guide](https://github.com/netbox-community/netbox/blob/develop/CONTRIBUTING.md) to get started.
- [Share your idea](https://plugin-ideas.netbox.dev/) for a new plugin, or [learn how to build one](https://github.com/netbox-community/netbox-plugin-tutorial) yourself!

## Project Stats

[](https://github.com/netbox-community/netbox#project-stats)

[![Timeline graph](https://camo.githubusercontent.com/f6e51afab3ac05569833b59d721b2859292fc69999d560dd18dc566960ee7df6/68747470733a2f2f696d616765732e7265706f6772617068792e636f6d2f32393032333035352f6e6574626f782d636f6d6d756e6974792f6e6574626f782f726563656e742d61637469766974792f7768517445725f544744395068573142506c686c4551356a6e726751304b4a706d2d4c6c4774706f474f302f334b785f6957555342524a352d41493451774a454a57725544457a334b7258326c766838615945305758595f74696d656c696e652e737667)](https://github.com/netbox-community/netbox/commits) [![Issues graph](https://camo.githubusercontent.com/01f048e111784448671d5f6dd93b1122d24bc911973b4440c1f6c44dc53a5ffb/68747470733a2f2f696d616765732e7265706f6772617068792e636f6d2f32393032333035352f6e6574626f782d636f6d6d756e6974792f6e6574626f782f726563656e742d61637469766974792f7768517445725f544744395068573142506c686c4551356a6e726751304b4a706d2d4c6c4774706f474f302f334b785f6957555342524a352d41493451774a454a57725544457a334b7258326c766838615945305758595f6973737565732e737667)](https://github.com/netbox-community/netbox/issues) [![Pull requests graph](https://camo.githubusercontent.com/6f2c7a57320452d083b6cdaa5b589f902328ec7d60f94e5411317160fba3ea78/68747470733a2f2f696d616765732e7265706f6772617068792e636f6d2f32393032333035352f6e6574626f782d636f6d6d756e6974792f6e6574626f782f726563656e742d61637469766974792f7768517445725f544744395068573142506c686c4551356a6e726751304b4a706d2d4c6c4774706f474f302f334b785f6957555342524a352d41493451774a454a57725544457a334b7258326c766838615945305758595f7072732e737667)](https://github.com/netbox-community/netbox/pulls) [![Top contributors](https://camo.githubusercontent.com/478a6e8889d14bfc391d9a1da13ddf92cbd7a7f45971ea858026e1bc9dd2888a/68747470733a2f2f696d616765732e7265706f6772617068792e636f6d2f32393032333035352f6e6574626f782d636f6d6d756e6974792f6e6574626f782f726563656e742d61637469766974792f7768517445725f544744395068573142506c686c4551356a6e726751304b4a706d2d4c6c4774706f474f302f334b785f6957555342524a352d41493451774a454a57725544457a334b7258326c766838615945305758595f75736572732e737667)](https://github.com/netbox-community/netbox/graphs/contributors)  
Stats via [Repography](https://repography.com/)

## Screenshots

[](https://github.com/netbox-community/netbox#screenshots)

**NetBox Dashboard (Light Mode)**  
[![NetBox dashboard (light mode)](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/home-light.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/home-light.png)

**NetBox Dashboard (Dark Mode)**  
[![NetBox dashboard (dark mode)](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/home-dark.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/home-dark.png)

**Prefixes List**  
[![Prefixes list](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/prefixes-list.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/prefixes-list.png)

**Rack View**  
[![Rack view](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/rack.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/rack.png)

**Cable Trace**  
[![Cable trace](https://github.com/netbox-community/netbox/raw/develop/docs/media/screenshots/cable-trace.png)](https://github.com/netbox-community/netbox/blob/develop/docs/media/screenshots/cable-trace.png)