---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---

# HOW TO RENAME AN ACTIVE DIRECTORY DOMAIN

written by [Cyril Kardashevsky](https://theitbros.com/author/administrator/) February 8, 2024

Changing the name of an Active Directory domain is something few AD administrators have ever done. The domain renaming process itself is fairly straightforward, but needs to be carefully planned so as not to break the entire corporate infrastructure.

The need to change an AD domain name usually arises in the context of a corporate acquisition, rebranding, M&A consolidation of multiple business units.

## Preparation for an AD Domain Rename

Before you proceed to change your domain name, check the basic requirements:

- [AD schema version](https://theitbros.com/upgrading-active-directory-schema/) at least Windows Server 2003;
- If the domain Certificate Authority (CA) is deployed in domain, make sure it is properly prepared ([Prepare certification authorities for domain rename](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816587(v=ws.10)?redirectedfrom=MSDN));
- Domains with on-premises Exchange Server (except 2003 versions) are incompatible with domain renaming. Migrating your users, groups, and computers to a new AD forest with Exchange using ADMT is the only solution in this case;
- There are some other Microsoft and non-Microsoft applications that do not support domain renaming renames (check your application’s documentation);
- You need to create a primary DNS zone for the new domain name in your AD;
- If your DFS namespaces, redirected folders, [roaming user profiles](https://theitbros.com/configuring-windows-roaming-user-profiles-in-active-directory/), etc. are implemented in your infrastructure, gather all the relevant information for them and schedule a migration immediately after the domain name change.

In this post, we will rename an existing **contoso.com** domain with AD controllers running Windows Server 2019 to **theitbros.com**.

> **Note**. Be sure to [backup your AD](https://theitbros.com/backup-active-directory/) before you start renaming your domain.

The first step is to create a primary DNS zone for the new domain on your DNS server:

1. Connect to DC and open the **DNS Manager** console (dnsmgmt.msc);
2. Expand the **Forward Lookup Zones** node;
3. Select **New zone**;  
    ![change domain name active directory](https://theitbros.com/wp-content/uploads/2022/09/rename-active-directory-domain.png "change domain name active directory")
4. Create a new [primary AD-integrated zone](https://theitbros.com/active-directory-integrated-dns-zone/) called **theitbros.com** with enabled **Allow only secure dynamic updates** option. Wait for the new zone to replicate all the DNS servers in the forest.  
    ![active directory change domain name](https://theitbros.com/wp-content/uploads/2022/09/changing-domain-name-active-directory.png "active directory change domain name")

## Renaming AD Domain Using RenDom Tool

In order to change the AD domain name, you must use the **rendom** console tool, which is available on any domain controller. The C:\Windows\System32\rendom.exe command allows you to perform all the necessary actions for a domain renaming operation.

Sign-in to the DC and open the command prompt as an administrator.

Run the following command to generate an XML file containing your domain configuration:

rendom /list

Open the **Domainlist.xml** with notepad:

notepad Domainlist.xml

![rename active directory domain](https://theitbros.com/wp-content/uploads/2022/09/rename-ad-domain.png "rename active directory domain")

Use the **Edit** > **Replace** option to find the old domain name in the file and replace it with the new one. Manually change the value in the NetBiosName field.

![change active directory domain name](https://theitbros.com/wp-content/uploads/2022/09/active-directory-rename-computer.png "change active directory domain name")

Save the changes to the Domainlist.xml file.

Verify the new configuration (command makes no changes yet):

rendom /showforest

![change ad domain name](https://theitbros.com/wp-content/uploads/2022/09/rename-active-directory-user.png "change ad domain name")

Upload a new configuration file to the DC running the Domain Naming Operations Master [FSMO](https://theitbros.com/fsmo-roles/) role:

rendom /upload

Wait for the file containing the domain renaming instructions to be replicated to all other domain controllers in the forest. You can force the synchronization of changes made on the Domain Naming Master to all DCs:

repadmin.exe /syncall /d /e /P /q DomainNamingMaster_DC_HostName

This creates a DCclist.xml file that is used to track the progress and status of each domain controller in the forest for the domain rename operation. At this point, the Rendom freezes your [Active Directory forest](https://theitbros.com/raise-domain-and-forest-functional-level/) from making any changes to its configuration (such as adding/removing DCs, configuring domain trusts, etc.).

Check if the domain is ready to accept changes (checks the availability of all DCs):

rendom /prepare

If this command returns no errors, you can run the rename operation:

rendom /execute

This command automatically reboots all domain controllers.

All the domain-joined workstations and member servers must be rebooted twice for the changes to take effect. The first reboot allows the domain member to detect the domain change and change the full computer name. The second is used to register the new computer name in the new DNS zone.

> **Note**. If there are any remote computers that [connect to your domain via VPN](https://theitbros.com/join-domain-and-login-over-a-vpn-connection/), you will need to unjoin them from the old domain and rejoin the new domain.

Now your users can log on to computers using their old usernames and passwords.

After that, you need to manually rename all domain controllers (they won’t automatically change their names to reflect the new domain).

Use the following command to rename each DC:

netdom computername DC01.contoso.com /add:DC01.theitbros.com

netdom computername DC01.contoso.com /makeprimary:DC01.theitbros.com

Reboot the domain controller to apply the changes.

![how to change domain name in active directory](https://theitbros.com/wp-content/uploads/2022/09/rename-active-directory.png "how to change domain name in active directory")

Now you need to rebind the Group Policy Objects to the new domain name:

gpfixup /olddns:contoso.com /newdns:theitbros.com

Then run the command to fix the NetBIOS name of the domain in the GPOs:

gpfixup /oldnb:CONTOSO /newnb:THEITBROS

Remove links to the old domain:

rendom /clean

You can now complete the domain rename and unfreeze the AD forest:

rendom /end

Make sure that the rename was successful. Check if all [Active Directory domain controllers can be contacted](https://theitbros.com/active-directory-domain-controller-could-not-be-contacted/), users can sign in to the new domain; check if applications work correctly, and [check AD replication](https://theitbros.com/check-active-directory-replication/) and errors on DCs.

Now change paths in DFS namespaces, roaming profiles, redirected folders, etc. if used.