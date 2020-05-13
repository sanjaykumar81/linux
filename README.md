# My Linux learning

## Viewing information on any topic

- man page are the most common way to view information about any given topic

```bash
man ls
```

- Update the db of man pages by running the `mandb` command. `mandb` Create or update the manual page index caches

```bash
mandb
```

- `whatis` (man -f) is another command to find quick definition of any command

```bash
man -f ls
whatis ls

```

- `apropos` (man -k) is another command to find quick info about any command

```bash
man -k ls
apropos ls
```

- `pinfo` give full manual about a given command. `pinfo` does not have manual about each command

```bash
apropos ls
```

- `man -K` search all places in the manual pages to find the reference of the given keyword.

- last but not the least `http://tldp.org/` is the good place to find any documentation.


## Searching files

- `grep`

```bash
grep -ir <key> <file|path> # i = case insensitive,  r = recursive
```

- `find` search for the file actually by traversing the disk

```bash
find <folder> -iname <filename> -type <f|d> -mtime +1
```

- `locate` is the command to find any file in the system.
    - steps to setup the `locate` command
    - First, install `mlocate`
    - Second, run `updatedb` command
    - Third, search the file `locate <file>`

## File directory

```bash 
man 7 hier # list all key folder under root directory
```


# User and Groups

## User

- Add user

```bash
useradd <username>
```

- Add or Change password for user

```bash
passwd <username>
```

- Add group to user

```bash
usermod -gG  <groupname> <username>
```
    
- Lock user

```bash
useradd -L <username>
```

- Un Lock user

```bash
useradd -U <username>
```

## Groups

- Add group

```bash
groupadd <groupname>
```

## File or Directory Permissions 

`ugo` = user group other

### Alphabetic way

- `+` will add the permission to existing permissions
- `-` will remove the permission from existing permissions
- `=` will set the permission. remove any existing permissions.

```bash
chmod a=x file # set the value for all to x=execute
chmod g+wx file  # set permission for group to wx (write and execute)
chmod u=rw # set permission of user to rw (read and write). Remove all permission of group and others.
chmod ugo=rw # set permission to read and write for user, group and others.
```

### Numeric way

- Numeric value is calculated based on the binary value of rwx 
- `r` = 100 (binary) = 4 (decimal)
- `w` = 010 (binary) = 2 (decimal)
- `r` = 001 (binary) = 1 (decimal)

- setting permission for user, group or other is sum of these numeric value

```bash
chmod 755 file #set rwx(4+2+1), r-x, r-x for user, group and other respectively
chmod 641 file #set rw-, r--, --x for user, group and other respectively
chmod 755 file #set rwx, r-x, r-x for user, group and other respectively
chmod 755 file #set rwx, r-x, r-x for user, group and other respectively

```

## Default permission and umask

- `0666` is the default permission given to files when you create it.
- `0777` is the default permission given to folder when you create it.
- Default permission works along with `umask` key.
- Each user can have different `umask`
- Effective permission is the difference between the default permission minus the `umask` key
- `umask` can be used to set the umask of the user

```bash 
umask 0002 # this will set the default permission on file = 0664 and directory = 0775
umask 0055 # this will set the default permission on file = 0611 and directory = 0722

```

## Special permissions

- `Sticky` bit represent by `t` is set at `other` level on Directory.
Implies that items in directory can be deleted by owners only.

```bash
ls -ld /tmp
drwxrwxrwt 4 root root 4096 May  4 21:17 /tmp
```

- `SGID` bit represent by `s|S` is set at `group` level on Files(executable) and Directory.
Implies that new object created in the directory inherit is group property of directory and not of the person creating the file.
In case of file(executable) then it run with the group permission of the file and not that of the invoker.

```bash
sudo find / * -type d -perm /2000 # to find the directory having sgid bit set
sudo find / * -type f -perm /2000 # to find the files having sgid bit set
```

- `SUID` bit represent by `s|S` is set at `user` level on files.
Applicable to executable. Program us with the privilege of owner in place of the executor.


```bash
sudo find / * -type d -perm /4000 # to find the binaries having suid bit set
```

## File ACL

- `getfacl` to get the file ACL

```bash
getfacl <filename>
```

- `setfacl` to set the file ACL

```bash
setfacl -m u:<username>:rwx <filename> # set rwx acl on file for user.
setfacl -m g:<grpname>:rw <filename> # set rw acl for the group on the file.

setfacl -x u:abc:rw testfile # remove the user acl 
setfacl -x g:grp:x testtwofile # remove the group acl
```



## Bash tricks

```bash
touch file-{a..c}-{1..30} # creats 90 files. {a..c} act as range in python
```

## File types

- `b`    block (buffered) special
- `c`    character (unbuffered) special
- `d`    directory
- `p`    named pipe (FIFO)
- `f`    regular file
- `l`    symbolic link; this is never true if the -L option or the -follow option is in effect, unless
 the symbolic link is broken.  If you want to search for symbolic links when -L is in  effect,
 use -xtype.
- `s`    socket
- `D`    door (Solaris)

## User home directory default files.

- When a user is created, a home directory is assigned to the user under the /home folder.
- Name of this directory is usually the name of the user. e.g /home/sanjay
- While creating this folder certain hidden files too are copied into it.
- e.g. .bashrc .bash_profile or .profile etc.

- List of file which are copied are from the `/etc/skel` folder.

## Process controlls
- `kill <signal> <pid>`  to send a signal to a process
- `killall <signal> <pname>` to send signal to a process by name
- `pkill <signal> <pid|pname|group|user etc>` send singal 
- `pgrep <regex name>` pgrep looks through the  currently  running  processes  and lists  the process IDs which match the selection criteria to stdout.
- `nohup` no hangup  is use to keep running the child process even when parent process is killed.

## Process priority 

- `nice` and `renice` are the command to set the process priority.
- `nice` value range from `-20` to `19`. Default nice value is `0`
- Priority of process is defined by fall in the range of 0-39. Default value is 20. Priority of a process can be calculated by subtracting nice from the default priority of 20.


## Input Output redirection

| Name | channel| Default| redirect| redirect append|
|------|--------|--------|---------|----------------|
|stdout| 1      |terminal| 1> or > | 1>>  or >>     |
|stderr| 2      |terminal| 2>      | 2>>            |
| stdin| 0      |keyboard| <       | <<AnyKeyWord(EOF)|

## Useful command list

## Files

- man 1 id
- man 1 getent
- man 1 passwd
- man 5 passwd
- man 1 users
- man 8 useradd
- man 8 userdel
- man 8 usermod
- man 8 vipw
- man 1 who
- man 1 alias
- man 1 cp
- man 1 mkdir
- man 1 mv
- man 1 rm
- man 1 rmdir
- man 1 touch
- man 7 glob
- man 7 hier
- man 1 cd
- man 1 chmod

## User and Group

- man 1 id
- man 1 getent
- man 1 gpasswd
- man 1 groups
- man 5 group
- man 5 shadow
- man 8 groupadd
- man 8 groupdel
- man 8 groupmod
- man 8 vigr
- man 1 su
- man 8 sudo
- man 5 sudoers
- man 8 visudo
- man 1 chage
- man 1 getent
- man 5 shadow
- man 8 usermod
- man 8 vipw
- man 1 umask
- man 1 getfacl
- man 1 setfacl
- man 5 acl

## Process

- man 1p bg
- man 1p fg
- man 1p jobs
- man 1 ps
- man 1p ps
- man 1 pstree
- man 1 sar
- man 1 top
- man 1 uptime

## Controlling process

- man 1 kill
- man 1p kill
- man 1 killall
- man 1 nohup
- man 1p nohup
- man 1 pkill
- man 7 regex
- man 7 signal

## Process Priorities

- man 1 top
- man 1 nice
- man 1p nice
- man 1 renice
- man 1p renice

## I/O Redirection & Piping

- man 1 cat
- man 1p cat
- man 1 cut
- man 1p cut
- man 1 mail
- man 1p tr
- man 4 null
- man 7 pipe
- man 1 sort
- man 1p sort
- man 1 tr
- man 1p tr

## Package Managers

- man 8 rpm
- man 8 yum
- man 5 yum.conf
- man 1 yum-config-manager

##  Storage & Swap

man 8 blkid
man 8 fdisk
man 5 fstab
man 8 gdisk
man 8 lsblk
man 8 mount
man 8 mkfs
man 8 mkfs.xfs

## Services & Boot

- man|apropos systemd
- man 1 sulogin
- man 1 systemctl
- man 5 systemd.target
- man 5 systemd.unit

## Networking

- apropos ip- ;-)
- man 8 ethtool
- man 1 hostname
- man 5 hostname
- man 7 hostname
- man 1 hostnamectl
- man 8 ip
- man 8 ip-address (ifconfig deprecated)
- man 8 ip-link
- man 8 ip-route (route deprecated)
- man 8 mtr
- man 8 ping
- man 8 ss (netstat old)
- man 8 tracepath
- man dig (nslookup deprecated)
- man 1 nmcli
- man 1 vimdiff 
- man 1 firewall-cmd
- man 1 firewalld
- man 5 firewalld-zones 

## System Logging 

### old way

- man 1 dmesg
- man 1 logger
- man 5 rsyslog.conf
- man 1 zcat
- man 1p zcat
- man 1 zgrep

#### new way

- man 1 journalctl
- man 8 systemd-journald.service
- man 8 systemd.journal-fields
