# Basics

- Active Directory is a central database that stores user account information, groups, computer accounts, group policies.
- A Domain Controller is a server that runs a replica of AD.
- LDAP is the protocol to query AD.
- Computer accounts have `$` at the end of the name.
- SYSVOL is a domain-wide share in AD that contains things like logon scripts and group policy data.
- Local Administrator account: `RID 500`.
- Passwords can be found in the Security Account Manager (SAM) `%SystemRoot%/system32/config/SAM`, `ntds.dit` (AD) or `lsass.exe`.


## NTLM

- By default AD uses Kerberos for authentication. It falls back to NTLM if the client tries to connect using the IP address of the machine instead of the hostname.

### How it works

- User enters credentials (domain name, username and password hash).
- Client sends an authentication request (domain name and username) to server.
- Server replies with a challenge.
- Client sends back the challenge encrypted with the password hash.
- Server sends the credentials and the response to the DC (If AD is not configured the check is done locally)
- DC verifies the informations and sends the result to the server.

# Abusing Active Directory

## LAN Manager hash

- Legacy Windows authentication hash which is sometimes used for backward compatibility.
- Passwords are not case sensitive. All passwords are converted into uppercase before generating the hash value.
- Password length is limited to maximum of 14 characters.
- A 14-character password is broken into 7+7 characters and the hash is calculated for the two halves separately.
- If the password is 7 characters or less, then the second half of hash will always produce the LM hash of an empty string (0xAAD3B435B51404EE) which makes a password length of 7 characters or less easily identifiable.
- The hash value is sent to network servers without salting.

## Directory Services Restore Mode account

- Local administrator account found in every DC, its password is rarely changed.

## Pre-Windows 2000 Compatible Access Group

- `Pre-Windows 2000 Compatible Access Group` grants `Everyone` the ability to browse through the AD. In case of an upgrade the group can have ANONYMOUS login enabled.

### Mitigation

- Remove the Everyone group from the Pre-Windows 2000 Compatible Access group.
- Ensure ANONYMOUS login is disabled.

## NTLM-Relay

- Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) are components of Microsoft Windows systems that are alternate methods of host identification when DNS fails.
- An attacker can respond to LLMNR UDP/5455 or NBT-NS UDP/137 broadcasts pretending that they know the location of the requested host (MITM).
- The attacker can trick the target into sending their username and password in the form of an NTLMv2 or v1 hash.

### Further details

- [Responder](https://github.com/SpiderLabs/Responder)

## Pass-the-hash

- In most cases getting the NT hash of a user is the same thing as getting his plaintext password, it can be used to execute commands using Impacket's `psexec.py` for example.

## Group Policy Preferences

- Released with Windows Server 2008, brought the ability to store and use credentials.
- Automates changing the password of the local Administrator account to avoid using scripts that can be read by unauthorized users.
- When a new GPP is created, an associated XML file is created in SYSVOL in which passwords are AES-256 bit encypted.
- [Microsoft is legally compelled to publish the AES key used by GPP to encrypt credentials](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN).
- Any authenticated user can search SYSVOL for "encrypted" passwords
```powershell
findstr /S /I cpassword \\<FQDN>\sysvol\<FQDN>\policies\*.xml
```
### Further details

- [Finding Passwords in SYSVOL - ADSecurity](https://adsecurity.org/?p=2288)

## Persistence

- AD snapshot
- Extended persmissions (Self-Membership, User-Force-Change-Password ...)
- Golden/Silver ticket

# Securing Active Directory

## Avoid password reuse with LAPS

## Manage Windows updates in a network's computer using WSUS

## Restrict domain Administration logon

## Network segmentation (MS-tier model)

## PCA/PRA