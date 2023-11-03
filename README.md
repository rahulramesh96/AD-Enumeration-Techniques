# AD-Enumeration-Techniques
AD enumeration techniques, commands and methodology for OSCP

If port 88 is open, it is usually a domain controller. This port is reposible for kerberos authentication. Before going ahead, make sure if port 88/tcp kerberos-sec is open.

## SMB

**SMB** ports are **139** and **445**.

#### 1. Run SMBMAP
`smbmap -H <IP>` # See if you can access any shares?

#### 2. Run SMBCLIENT
`smbclient -L <IP>` # See if you have access to any shares?

`smbclient -L <IP> -U ''` # See if you can login with null authentication

## RPC

The service name would look like **msrpc** on port **135**

#### 3. Run RPCCLIENT
`rpcclient -U '' -N <IP>`

If you see something like below:
**`rpcclient $>`**

Enter the following in the shell:
`rpcclient $> enumdomusers`

You will get the list of users from the command.

## LDAP

The service name would be **ldap** on port **389** or **636** or **3268**

#### 4. Run LDAPSEARCH

The below *htb* and *local* strings are the domain names. Perform an Nmap (-sV) version scan on the host for all the ports. In the LDAP ports, we see the domain names. Enter them separated by a '**,**'. Use **DC=** for all the domains separated by **'.'** 

Example AD Domain: offsec.htb.local
"DC=offsec,DC=htb,DC=local"

`ldapsearch -H ldap://<IP> -x -b "DC=offsec,DC=htb,DC=local"`

If the above command gives you a bunch of data, run the command below ot ge tinformation about the users in the domain

`ldapsearch -H ldap://<IP> -x -b "DC=offsec,DC=htb,DC=local" '(objectClass=User)' "sAMAccountName" | grep sAMAccountName`









