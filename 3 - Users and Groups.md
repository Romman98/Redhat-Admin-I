# Permission

A file permission is split into 3 groups. Each group is split into 3 bits.

\--- --- --- 

The first octet is for the user level. \
The second octect is for the group level. \
The third octect is for the others.

For example:
```
rwx rwx rwx # this means that user, user group, and others have full permission.
rwx r-- --- # This means that the user has full permission, the user group has read only, and others have no permission.
```

each permission is counted as 1 and no permission is counted 0. `rw-` is translated to `110` which is 6. `r--` is translated to `100` which means 4. So a full permission `rwxrwxrwx` means `777`.


## Change permission

To change permission for a file we use the command `chmod`.

### On an octect level

```
chmod u+w file_name
chmod g-r file_name
```

### Permission level

```
chmod +x file_name
chmod -w file_name
```

### All at once

```
chmod 644 file_name
chmod 777 file_name
```

### Note

Directories need `x` permission to be able to cd into them. 

# Users and Group Management

## Users

In linux there are 3 types of users.

1. Superuser
2. Regular
3. System

### Superuser

It has full permission to do whatever it wants. The superuser is called `root`. Be careful with who has access to this user. It is installed by default when you install the OS.

### Regular

Regular users are the standard users. They have their own home directories under `/home` and they can create files and directories in it. They can perform standard tasks based on the permission system.

### System

Installed services sometimes are installed with their own user. You don't use it to login, but they are used for these services to interact with the system and to stay isolated.


## /etc

In the older days of linux, passwords were saved in `/etc/passwd`. Later, more security concerns were raised and a new architecture was placed.

### /etc/passwd

`/etc/passwd` is still used in RHEL and other distributions. It contains many information about the user.

| Field       | Description                                                                                         |
| ----------- | --------------------------------------------------------------------------------------------------- |
| User        | This field contains the username                                                                    |
| Password    | This field used to store information about the password, but now it has been moved to `/etc/shadow` |
| UID         | The User ID                                                                                         |
| GID         | The Group ID                                                                                        |
| User Info   | Optional Description about the user                                                                 |
| Home Dir    | The home directory of the user. By default it is `/home/username`                                   |
| Login Shell | The login shell of the user. By default it is `/bin/bash`.                                          |

![](./images/etc_passwd.png)

### /etc/shadow

This file contains more information about the password of each user. There must be an `x` in the password field in `/etc/passwd` for the user to have a record in this file.

| Field                         | Description                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------- |
| User                          | This field contains the username                                              |
| Password                      | The encrypted password                                                        |
| Date of last password changed | in days after 1970-01-01                                                      |
| minimum password age          | Number of days that the password must stay before changing it                 |
| Maximum password age          | Number of days until the password must be changed                             |
| Password warning period       | Number of days before a warning is given to change the password               |
| Password inactivity period    | Number of days after password expiration which the password is still accepted |
| Account expiration date       | Number of days after which the password is expired, since 1970-01-01          |
| Reserved field                | Reserved field for future use                                                 |

In the password field, $6 means that the password is protected using the 512-bit secure hash algorithm (SHA-512). And a `*` means that this user has no password. You cannot login using this user.

![](./images/etc_shadow.png)

### /etc/group

Every user has a primary group. This is created when the user is created. A user can also have a supllementary group. some groups are special. For example, the `wheel` group allows the user to use the sudo command. 

| Field          | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| Group Name     | The name of the group                                                    |
| Group password | Group password. `x` means that there is a record of it in `/etc/gshadow` |
| Group ID       | Group ID                                                                 |
| Group Members  | A comma-seperated list of all the memebers of this group.                |

![](./images/etc_group.png)


### /etc/gshadow

It is the configuration file of the groups. It has the owner/administrator of each group whhich can add memebrs using `gpasswd` 

| Field         | Description                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| Group Name    | Group Name                                                                                                        |
| Password      | If the group has a password, you will find the hashed password. Otherwise, it is just `!` which means no password |
| Administrator | The administrator of the group who can add and remove members.                                                    |
| Group Members | A comma-seperated list of all the memebers of this group.                                                         |

![](./images/etc_gshadow.png)

## Add and Delete Users

To add user in linux we use the `useradd` command

```
useradd <username>
```

Options

| Option                  | Description                                      |
| ----------------------- | ------------------------------------------------ |
| -u UID                  | Sets a custom UID for the user                   |
| -g GID                  | Sets a custom GID for the user's group           |
| -c description          | Sets a description for the user in `/etc/passwd` |
| -d home_dir             | Overrides the default home directory             |
| -e user expiration date | Sets an expiration date for the user             |
| -G group1,group1        | Add the user to an existent group                |
| -s shell                | overrifes the default shell for the user         |


To delete a user, simply run `userdel <username>` but this will keep the home directory. You can add `-r` to delete the home directory as well.

```
userdel -r username
```

## Add and Deletes Groups

You can create groups other than the one created by default. It is recommended to assign it a GID different 

## Modifying Accounts

`usermod`, `groupmod`, `chage`

### usermod

Allows you to change the settings in `/etc/passwd` and `/etc/shadow`

- Append users to existing groups
- Rename the user
- Lock and unlock password 

`usermod` also supports all the options that are available for `useradd`.

note: When a user is locked a `!` appears in `/etc/shadow`

### groupmod

The command `groupmod` supports only two use cases:

1- It is used to change the GID

```
groupmod -g <GID> <group>
```

2- Changing thhe group name

```
groupmod -n <new_name> <old_name>
```

### chage

This command is used to manage aging information for the password.

| Option        | Description                                                                           |
| ------------- | ------------------------------------------------------------------------------------- |
| -d YYYY-MM-DD | Sets the last change date for the password                                            |
| -E YYYY-MM-DD | Assigns expiration date for an account                                                |
| -I `x`        | Locks an account `x` amount of days after a password has expired                      |
| -l            | Lists all age information                                                             |
| -m `x`        | minimum days that a user must keep a password                                         |
| -M `x`        | Sets the max days that a user is allowed to keep the password. -1 means never expired |
| -W `x`        | Days a user is warned to change their password.                                       |

