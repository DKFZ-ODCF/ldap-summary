# LDAP-summary

This repository contains useful shell scripts to make interacting with LDAP/AD from the command line more pleasant.

The shell scripts provide easy access to the most common types of information that we've needed on users/groups at the DKFZ.
They are made available here in the spirit of making every admin's life easier.
The scripts are wrappers around [`ldapsearch`](https://linux.die.net/man/1/ldapsearch) by the [OpenLDAP project](https://www.openldap.org/).

# Configuration

The scripts load their LDAP configuration from a separate config file: `ldap-summary.conf`, located in `$XDG_CONFIG_HOME` (by default: `$HOME/.config/`).
An example config file is provided as `ldap-summary.conf.template`, which contains dummy values for all required settings.

# Examples

## Basic LDAP info on a ...
### .. user
```
# ./ldap CN=johndoe
cn: johndoe
displayName: Doe, John
dn: CN=johndoe,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
mail: j.doe@example.com
memberOf: CN=Powerusers               OU=Department      full DN: CN=Powerusers,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
memberOf: CN=SomeProject              OU=Department      full DN: CN=SomeProject,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
memberOf: CN=TTP_project              OU=Department      full DN: CN=TTP_project,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
memberOf: CN=SCP_project              OU=Department      full DN: CN=SCP_project,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
...
memberOf: CN=storage-access           OU=Department      full DN: CN=storage-access,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
memberOf: CN=UserWithParking          OU=Company         full DN: CN=UserWithParking,OU=Company,DC=ad,DC=example,DC=com
ufn: johndoe, Department, Unit, Company, ad.example.com
```
### .. group
```
# ./ldap cn=SOME-GROUP
cn: SOME-GROUP
description: AD and NIS Group to control access to controlled data for SOME-PROJECT
dn: CN=SOME-GROUP,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
member: CN=ADMIN-GROUP              OU=Department      full DN: CN=ADMIN-GROUP,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
member: CN=janedoe                  OU=DefenseWeekly   full DN: CN=janedoe,OU=DefenseWeekly,OU=DefenseUnit,OU=Company,DC=ad,DC=example,DC=com
member: CN=broadwell                OU=GeorgiaState    full DN: CN=broadwell,OU=GeorgiaState,OU=Literature,OU=EduUnit,DC=ad,DC=example,DC=com
....
member: CN=aladin                   OU=Lamp            full DN: CN=aladin,OU=lamp,OU=Unit,DC=ad,DC=example,DC=com
ufn: SOME-GROUP, Department, Unit, Company, ad.example.com

```


## compare members between two groups
```
# ./groupdiff groupA groupB
groupA         groupB
=============  =============
---            troyhunt
aladin         aladin
samspade       samspade
---            pnorton
---            mcaffeej
```

## compare _multiple_ groups
```
# group-matrix group1 group2 ...
USERS     group1  group2  group3
troyhunt  Y
samspade  Y       Y       Y
pnorton   Y
mcaffeek  Y       Y
gatesb    Y
```

## _recursively_ find ALL groups of a user ##
This finds all groups of a user, including those that they are only indirectly a member of.
```
# ./groupsof <username>
# ./groupsof johndoe
 CN=Team                     OU=Department      dn: CN=Team,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
 CN=CITRIX FileZilla         OU=Citrix Server   dn: CN=CITRIX FileZilla,OU=Citrix Server,OU=Terminal Server,DC=ad,DC=example,DC=com
 CN=CITRIX putty             OU=Citrix Server   dn: CN=CITRIX putty,OU=Citrix Server,OU=Terminal Server,DC=ad,DC=example,DC=com
 CN=CITRIX WinSCP3           OU=Citrix Server   dn: CN=CITRIX WinSCP3,OU=Citrix Server,OU=Terminal Server,DC=ad,DC=example,DC=com
 CN=Terminal Server Users    OU=Citrix Server   dn: CN=Terminal Server Users,OU=Citrix Server,OU=Terminal Server,DC=ad,DC=example,DC=com
 CN=CITRIX Browser           OU=Citrix Server   dn: CN=CITRIX Browser,OU=Citrix Server,OU=Terminal Server,DC=ad,DC=example,DC=com
 CN=WLAN                     OU=RADIUS          dn: CN=WLAN,OU=RADIUS,DC=ad,DC=example,DC=com
 CN=UserWithParking          OU=Company         dn: CN=UserWithParking,OU=Company,DC=ad,DC=example,DC=com
 CN=Team-Data                OU=Department      dn: CN=Team-Data,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
... <MANY groups omitted>
 CN=Team-Data-SecretProject  OU=Department      dn: CN=Team-Data-SecretProject,OU=Department,OU=Unit,DC=ad,DC=example,DC=com
 CN=Team-viztools            OU=Team            dn: CN=Team-viztools,OU=Team,OU=Unit,DC=ad,DC=example,DC=com
```

# License and contribution

The scripts and example code in this project are licensed under the MIT license.
Any contributions intentionally submitted to this project will be governed by that same license.
