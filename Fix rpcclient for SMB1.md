# Fix rpcclient for SMB1

This guide is to fix an issue with Kali 2020 where rpcclient fails for SMB1

**Failure:**
`kali@kali:~% rpcclient -U "" 192.168.1.124
Cannot connect to server.  Error was NT_STATUS_CONNECTION_DISCONNECTED`

**Expected:** To see a prompt from rpcclient

`rpcclient $>` 

ref: [https://pramos.medium.com/fix-older-smb-kali-2020-4ee643ff29be](https://pramos.medium.com/fix-older-smb-kali-2020-4ee643ff29be)

---

The cause is not samba (smb.conf) not having a fallback protocol

## Fix smb.conf

[smb.patch](Fix%20rpcclient%20for%20SMB1%20183a9644bd0d4246a7cff83c2fc1b564/smb.patch)

1. make a copy of smb.conf
`sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bakup` 
2. open smb.conf file 
`sudo nano /etc/samba/smb.conf`
3. Goto line 30
or anywhere below the `[global]` 
should look like this:

    > `[global]
    ## Browsing/Identification ###
    # Change this to the workgroup/NT-domain name your Samba server will part of
    workgroup = WORKGROUP`
    `### Networking ####`

4. add `client min protocol = NT1`
so it looks like this:

    > `[global]
    ## Browsing/Identification ###
    # Change this to the workgroup/NT-domain name your Samba server will part of
    workgroup = WORKGROUP
    client min protocol = NT1
    #### Networking ####`

5. Hit Ctrl-X and save.

Now run the command: `rpcclient -U "" -N 192.168.1.124`

> kali@kali:~% sudo rpcclient -U "" -N 192.168.1.124
rpcclient $> querydominfo
Domain: WORKGROUP
Server: METASPLOITABLE
Comment: metasploitable server (Samba 3.0.20-Debian)
Total Users: 35
Total Groups: 0
Total Aliases: 0
Sequence No: 1614590370
Force Logoff: -1
Domain Server State: 0x1
Server Role: ROLE_DOMAIN_PDC
Unknown 3: 0x1
rpcclient $>