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

# Users

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