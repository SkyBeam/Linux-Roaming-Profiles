# Linux-Roaming-Profiles
A roaming profile implementation for Linux

# Abstract
This project targets to provide the tooling required to provide roaming user profiles similar to what Windows provides.

## Basic idea
* On user-login the user home directory (or parts of it) is synchronized from a remote server to the local disk.
* During user sessions the local profile provides quick access to files and is not prone to network interruptions.
* On termination of the last session the user profile is synchronized to a remote server.

## Advantages
Advantages over other technologies like NFS/SMB home mounts or similar technologies.
* Compared to mounted network shares this is providing better offline usability. Cache-FS and NFS/SMB cached operations might not be ideal for every use-case. Specifically when accessing less-frequently used files.
* Compared to fully local user profiles/home-directories it provides a way for automatic backup of user files and settings.
* Allow fully offline operation.

## Disadvantages
Disadvantages compared to other NFS/SMB home mounts or local home mounts.
* Logon/logoff delay: Large file sync might slow down logon/logoff processes.
* Risk of improper sync: Specifically when loging on offline or logging off while offline sync might be incomplete. Subsequent logon/logoff sequenences need to deal with such situations as well as possible.
* Multiple systems: When logging on to multiple systems at the same time conflicts might occur during synchronization to the master profile on server.
* Linux does not provide guidelines to application developers where to place "local-only" files and where to provide "roaming" profile date just as Windows does with "%AppData%" and "%LocalAppData%" folders. So it might be required to adjust a custom include/exclude list depending on applications used.

# Preconditions
Currently this projeöct is being validated on Manjaro Linux. It is focussing on synchronizing user profiles from/to an SMB server (preferrably Samba but can also be any Windows Server) to allow integration into Windows environments where typically Roaming Profiles are used. Authentication is focussing on Kerberos with Winbind to allow automatic single-sign-on and user-space profile synchronization.
Development target is to work with graphical sesssions as well as SSH sessions.

# Features
* Systemd unit assuring profile data to be pulled on login and pushed on logout.
* Script to be used with pam_exec to force early profile push while Kerberos credentials are still cached.
* Global exclude list allowing to exclude files from sync by pattern.
