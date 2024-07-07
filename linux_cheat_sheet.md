# Notes on making linux life more manageable  

## Alias 'shortcut commands'

The following command opens the .bashrc file to modify Aliases:
```bash
sudo nano .bashrc
```
You can substitute nano for vi, vim, emacs. Whatever is your text editor of 
choice.

Uncomment whatever aliasing you prefer my personal favorite is:
```bash
alias ls='ls -l --color=auto'
```

---

## Encrypting a and decrypting a file:
Encrypt a file.
```bash
gpg -c 'FileName.csv'
```

Decrypting a file:
```bash
gpg 'FileName.csv.gpg'
```

## Freeing up space on /boot partition:

Lists all kernel versions on the partition:
```bash
dpkg -l linux-image-\* | grep ^ii
```

Shows which kernel version you are currently using:
```bash
uname-r
```

Remove an old kernel version:

*NOTE:* Replace x.x.x-xx-generic with the version you want to remove.

```bash
sudo apt-get purge linux-image-x.x.x-xx-generic
```


## Linux sys admin: Adding users, groups, setting permissions and ownership.

Creating a user:
```bash
useradd 'username' -m -s /bin/bash -g(m$
```

Removing a user:
*NOTE:* The r tag removes the home directory of the user specified.
```bash
userdel -r 'username'
```

Removing a user:
*NOTE:* Removes the user but leaves the user's home directory. You would use 
this for data retention policies.
```bash
userdel 'username'
```

Changing your password:
```bash
passwd 'your_password'
```

Adding groups:
```bash
groupadd 'group_name'
```

Deleting groups:
```bash
groupdel 'group_name'
```

Adding users to groups:
```bash
gpasswd -a 'username' 'group_name'
```

Removing users from groups:
```bash
gpasswd -d 'username' 'group_name'
```

`who`: 

The `who` command tells you who is currently logged into the machine.

### chmod

The chmod command is used to change the permissions of a file or directory. To 
use it, you specify the desired permission settings and the file or files that 
you wish to modify. There are two ways to specify the permissions, but I am only
going to teach one way. It is easy to think of the permission settings as a 
series of bits (which is how the computer thinks about them). Here's how it 
works:

With files and directories a user or group can read(r), write(w), or execute(x)

```
rwx rwx rwx = 111 111 111
rw- rw- rw- = 110 110 110
rwx --- --- = 111 000 000
```

and so on...

```
rwx = 111 in binary = 7
rw- = 110 in binary = 6
r-x = 101 in binary = 5
r-- = 100 in binary = 4
```


chmod - modify file access rights

su - temporarily become the superuser

chown - change file ownership

chgrp - change a file's group ownership

---

## Navigating linux command line

The filesystem is GNU/Linux is like a tree, except that the root is on top.


```
/
  bin/
  home/
    user_name/
      Documents/
      Downloads/
      fileA.txt
      fileB.jpg
  usr/
  var/
```


If you want to move inside the tree, one option is to use relative paths. If 
you are in ```/home/user_name```, then typing cd Downloads will work,because 
Downloads is an immediate child of your current directory. If you are in the 
subfolder Documents and want to change directory ```cd``` to Downloads, you 
have to go up ```..``` and then to Downloads. So the correct command would be 
```cd ../Downloads```. You could also enter an absolute path. So the Downloads 
folder is a subfolder of ```user_name``` which is a subfolder of ```home``` 
which is ```…``` So you can also enter ```cd /home/user_name/Downloads```. 
Wherever you are in the filesystem. ```~``` always refers to the home directory 
of the current user ```/home/user_name``` in your case. If you enter 
```cd ~/Downloads``` you'll land in your Downloads folder.

    . refers to the current directory, so cd ./Downloads is roughly equivalent 
    to cd Downloads.

    .. means "parent directory".

    / at the beginning of file path refers to the root directory.

The next nice thing is tab expansion. If you enter ```cd ~/DowTab``` 
(last is pressing Tabulator key), the bash automatically expands it to 
```cd ~/Downloads```. This allows us to be able to navigate the file system more
quickly.


### Moving Around the Filesystem

Commands for moving around the filesystem include the following.

```pwd```: The ***pwd*** command Shows what directory you're currently in 
(pwd stands for "print working directory"). For example, ```pwd``` in the 
***desktop*** directory will show ```~/Desktop```. Note that the GNOME terminal 
also displays this information in the title bar of its window.

```cd```: The ***cd*** command allows you to change directories. When you open 
a terminal, you will be in your home directory. To move around the filesystem, 
use cd.

    •  To navigate to your desktop directory, use cd ~/Desktop

    •  To navigate into the root directory, use cd /

    •  To navigate to your home directory, use cd

    •  To navigate up one directory level, use cd ..

    •  To navigate to the previous directory (or back), use cd -

    •  To navigate through multiple levels of directories at once, use an 
    absolute path, for example: 
        cd /var/www
    will take you directly to the /www subdirectory of /var.

---

## Manipulating Files and Folders

You can manipulate files and folders by using the following commands.

### Copy a file:
---
```cp```: 

The `cp` (copy) command is used to copy files and directories. It can copy files
from one location to another, either within the same directory, to another 
directory, or even to another file system.
#### Basic Syntax:
```bash
cp [options] source destination

```
- `source`: The file or directory you want to copy.
- `destination`: The location where you want to copy the file or directory to.

#### Common Options:
- `-r` or `-R`: Recursively copy directories and their contents.
- `-i`: Interactive mode; prompts before overwriting files.
- `-u`: Copy only when the source file is newer than the destination file or 
when the destination file is missing.
- `-v`: Verbose mode; shows the files being copied.
- `-a`: Archive mode; preserves the structure and attributes of files and 
directories.
- `-f`: Force; if the destination file cannot be opened, remove it and try 
again.

#### Working Examples:
To copy a file named `file1.txt` to `file2.txt`:
```bash
cp file1.txt file2.txt
```

To copy `file1.txt` to the `/home/user/documents` directory:
```bash
cp file1.txt /home/user/documents/
```

To copy a directory named `dir1` and all its contents to `dir2`:
```bash
cp -r dir1 dir2
```

To copy file1.txt to file2.txt with a prompt before overwriting:
```bash
cp -i file1.txt file2.txt
```

To copy `file1.txt` to `file2.txt` and display the copy process:
```bash
cp -v file1.txt file2.txt
```

To copy `dir1` to `dir2` while preserving file attributes and structure:
```bash
cp -a dir1 dir2
```

To copy `file1.txt` to `file2.txt` only if `file1.txt` is newer:
```bash
cp -u file1.txt file2.txt
```
---



### Move a file:
```mv```: 

The `mv` (move) command in Linux is used to move files and directories from one 
location to another. It can also be used to rename files and directories.

#### Basic Syntax:

```bash
mv [options] source destination
```

- `source`: The file or directory you want to move or rename.
- `destination`: The new location or name for the file or directory.

#### Common Options:
- `-i`: Interactive mode; prompts before overwriting files.
- `-u`: Move only when the source file is newer than the destination file or 
when the destination file is missing.
- `-v`: Verbose mode; shows the files being moved.
- `-f`: Force; do not prompt before overwriting files.

#### Working Examples:
To move a file named `file1.txt` to `file2.txt`:
```bash
mv file1.txt file2.txt
```

To move `file1.txt` to the `/home/user/documents directory`:
```bash
mv file1.txt /home/user/documents/
```

To rename a directory named `dir1` to `dir2`:
```bash
mv dir1 dir2
```

To move `file1.txt` to `file2.txt` with a prompt before overwriting:
```bash
mv -i file1.txt file2.txt
```

To move `file1.txt` to `file2.txt` and display the move process:
```bash
mv -v file1.txt file2.txt
```

To move `file1.txt` to `file2.txt` only if `file1.txt` is newer:
```bash
mv -u file1.txt file2.txt
```
---

### Remove a file:
```rm```: 

The `rm` (remove) command in Linux is used to delete files and directories. It 
can remove files, multiple files, directories, and directories with their 
contents.

#### Basic Syntax:

```bash
rm [options] file_or_directory
```
- `file_or_directory`: The file or directory you want to delete.

#### Common Options:
- `-r` or `-R`: Recursively remove directories and their contents.
- `-i`: Interactive mode; prompts before each removal.
- `-f`: Force; ignore nonexistent files and never prompt.
- `-v`: Verbose mode; shows the files being removed.

#### Working Examples:

To remove a file named `file1.txt`:
```bash
rm file1.txt
```

To remove multiple files `file1.txt`, `file2.txt`, and `file3.txt`:
```bash
rm file1.txt file2.txt file3.txt

```

To remove an empty directory named `dir1`:
```bash
rm -d dir1
```

To remove a directory named `dir1` and all its contents:
```bash
rm -r dir1
```

To remove `file1.txt` with a prompt before removing:
```bash
rm -i file1.txt
```

To forcefully remove `file1.txt` without prompting:
```bash
rm -f file1.txt
```

To remove `file1.txt` and display the removal process:
```bash
rm -v file1.txt
```

---

### List files in a directory:
```ls```:

The `ls` (list) command in Linux is used to list the contents of a directory. 
It can display files, directories, and various information about them.

#### Basic Syntax:

```bash
ls [options] [file_or_directory]
```

- `file_or_directory`: The file or directory you want to list. If omitted, `ls`
lists the contents of the current directory.

#### Common Options:
- `-l`: Use a long listing format.
- `-a`: Include all entries, including hidden files 
(those starting with a dot .).
- `-h`: Human-readable format; with `-l`, print sizes in human-readable format
(e.g. 1K, 234M, 2G).
- `-r`: Reverse order while sorting.
- `-t`: Sort by modification time, newest first.
- `-S`: Sort by file size, largest first.
- `-R`: Recursively list subdirectories encountered.

#### Working Examples:
To list the contents of the current directory:
```bash
ls
```

To list the contents of the current directory in long format:
```bash
ls -l
```

To list all files, including hidden files, in the current directory:
```bash
ls -a
```

To list files in long format with human-readable file sizes:
```bash
ls -lh
```

To list files sorted by modification time, newest first:
```bash
ls -lt
```

To list files sorted by size, largest first:
```bash
ls -lS
```

To list all files in the current directory and all its subdirectories:
```bash
ls -R
```

To list all files, including hidden files, in long format with human-readable 
sizes:
```bash
ls -alh
```

---
### make a new directory:
```mkdir```: 

The `mkdir` (make directory) command in Linux is used to create directories. It 
can create single or multiple directories at once, including parent directories 
if necessary.

### Basic Syntax

```bash
mkdir [options] directory_name
```
- `directory_name`: The name of the directory you want to create.

#### Common Options
- `-p`: Create parent directories as needed.
- `-v`: Verbose mode; print a message for each created directory.
- `-m`: Set the file mode (permissions) for the created directories.

#### Working Examples:

To create a directory named `dir1`:
```bash
mkdir dir1
```

To create multiple directories `dir1`, `dir2`, and `dir3`:
```bash
mkdir dir1 dir2 dir3
```

To create a directory along with its parent directories (e.g., `parent/child`):
```bash
mkdir -p parent/child
```

To create a directory named `dir1` and print a message:
```bash
mkdir -v dir1
```

To create a directory named `dir1` with specific permissions (e.g. `755`):
```bash
mkdir -m 755 dir1
```

To create parent directories with verbose output and specific permissions:
```bash
mkdir -pv -m 755 parent/child
```

---

### Modify permissions of a file or directory:
```chmod```:

The `chmod` (change mode) command in Linux is used to change the file mode 
(permissions) of a file or directory. It can modify the read, write, and execute
permissions for the owner, group, and others.

#### Basic Syntax

```bash
chmod [options] mode file_or_directory
```

- `mode`: The new permissions for the file or directory.
- `file_or_directory`: The file or directory for which to change the 
permissions.

#### Common Options:
- `-R`: Recursively change permissions for all files and directories within a 
directory.
- `-v`: Verbose mode; output a diagnostic for every file processed.
- `-c`: Like verbose but report only when a change is made.
- `--reference=RFILE`: Use RFILE's mode instead of MODE values.
Permission Symbols
Permissions can be set using symbolic or numeric mode.

#### Symbolic Mode:
- `r`: Read permission.
- `w`: Write permission.
- `x`: Execute permission.
- `u`: User (owner).
- `g`: Group.
- `o`: Others.
- `a`: All (user, group, and others).

#### Numeric Mode:
- `4`: Read permission.
- `2`: Write permission.
- `1`: Execute permission.
- `0`: No permission.

The permissions are combined to form a three-digit octal number representing the
owner, group, and others, respectively.

#### Working Examples:

To set permissions to `755` for `file1.txt`:
```bash
chmod 755 file1.txt
```
This gives read, write, and execute permissions to the owner, and read and 
execute permissions to the group and others.

To add execute permission for the user, group, and others to `file1.txt`:
```bash
chmod a+x file1.txt
```

To remove write permission for the group and others from `file1.txt`:
```bash
chmod go-w file1.txt
```

To recursively set permissions to `755` for `dir1` and all its contents:
```bash
chmod -R 755 dir1
```

To set permissions to `644` for `file1.txt` and display the change:
```bash
chmod -v 644 file1.txt
```

To set the permissions of `file2.txt` to match those of `file1.txt`:
```bash
chmod --reference=file1.txt file2.txt
```

---

### Change Ownership of a file or directory:
```chown```:

The `chown` (change owner) command in Linux is used to change the ownership of a
file or directory. It can change both the user and group ownership.

#### Basic Syntax:

```bash
chown [options] user[:group] file_or_directory
```

- `user`: The new owner of the file or directory.
- `group`: The new group of the file or directory (optional).
- `file_or_directory`: The file or directory for which to change the ownership.

#### Common Options:
- `-R`: Recursively change ownership for all files and directories within a 
directory.
- `-v`: Verbose mode; output a diagnostic for every file processed.
- `-c`: Like verbose but report only when a change is made.
- `--reference=RFILE`: Use RFILE's ownership instead of specifying a new owner 
and group.

#### Working Examples:

To change the owner of `file1.txt` to `newuser`:
```bash
chown newuser file1.txt
```

To change the owner to `newuser` and the group to `newgroup` for `file1.txt`:
```bash
chown newuser:newgroup file1.txt
```

To change the group of `file1.txt` to `newgroup`:
```bash
chown :newgroup file1.txt
```

To recursively change the owner to `newuser` for `dir1` and all its contents:
```bash
chown -R newuser dir1
```

To change the owner of `file1.txt` to `newuser` and display the change:
```bash
chown -v newuser file1.txt
```

To set the ownership of `file2.txt` to match that of `file1.txt`:
```bash
chown --reference=file1.txt file2.txt
```

---

## System Information Commands

`df`:

The `df` command displays filesystem disk space usage for all partitions. The 
command `df-h` is probably the most useful. It uses megabytes (M) and gigabytes 
(G) instead of blocks to report. (-h means "human-readable.")

`free`:

The `free` command displays the amount of free and used memory in the system. 
For example, free -m gives the information using megabytes, which is probably 
most useful for current computers.

`top`:

The `top` command displays information on your Linux system, running processes, 
and system resources, including the CPU, RAM, swap usage, and total number of 
tasks being run. To exit top, press Q.

`uname -a`:

The `uname` command with the `-a` option prints all system 
information, including machine name, kernel name, version, and a few other 
details. This command is most useful for checking which kernel you're using.

`lsb_release -a`:

The `lsb_release` command with the `-a` option prints version information for 
the Linux release you're running.

For example:
```
user@computer:~$ lsb_release -a
LSB Version: n/a
Distributor ID: Ubuntu
Description: Ubuntu (The Breezy Badger Release)
Release:
Codename: breezy
```


`ifconfig`:

This reports on your system's network interfaces.

`iwconfig`:

The `iwconfig` command shows you any wireless network adapters and the 
wireless-specific information from them, such as speed and network connected.

`ps`:

The `ps` command allows you to view all the processes running on the machine.

---

## Hardware Information Commands
The following commands list the hardware on your computer, either of a specific 
type or with a specific method. They are most useful for debugging when a piece 
of hardware does not function correctly.

`lspci`:

The `lspci` command lists all PCI buses and devices connected to them. This 
commonly includes network cards, sound cards, graphics cards, RAID controllers.

`lsusb`:

The `lsusb` command lists all USB buses and any connected USB devices, such as 
printers and thumb drives.

`lshal`:

The `lshal` command lists all devices the hardware abstraction layer (HAL) knows
about, which should be most hardware on your system.

`lshw`:

The `lshw` command lists hardware on your system, including maker, type, and 
where it is connected.

---

## Searching and Editing Text Files

`grep`:

The `grep` command allows you to search inside a number of files for a 
particular search pattern and then print matching lines. 

For example:
```bash
grep blah file1.txt
```
will search for the text "blah" in the file and then print any matching lines.


`sed`:

The `sed` (or Stream EDitor) command allows search and replace of a particular 
string in a file.

For example if you want to find the string "cat" and replace it with "dog" in a 
file named pets:
```bash
sed s/cat/dog/g pets.
```

`cat`:

The `cat` command, short for concatenate, is useful for viewing and adding to 
text files. 

Displays the contents of the file:
```bash
cat file1.txt
```

Adds the contents of the `file1.txt` to `file2.txt`.
```bash
cat file1.txt file2.txt
```

`less`:

The `less` command is used for viewing text files as well as standard output. A 
common usage is to pipe another command through less to be able to see all the 
output
EG:
```bash
ls | less
```

---

## Using Wildcards:

Sometimes you need to look at or use multiple files at the same time. For 
instance, you might want to delete all .rar files or move all .odt files to 
another directory. Thankfully, you can use a series of wildcards to accomplish 
such tasks.

`*` matches any number of characters. For example, `*.rar` matches any file with
the ending `.rar`.

`?` matches any single character. For example, `?.rar` matches `a.rar` but not 
`ab.rar`.

`[characters]` matches any of the characters within the brackets. For example, 
`[ab].rar` matches `a.rar` and `b.rar` but not `c.rar`.

`[!characters]` matches any characters that are not listed. For example, 
`[!ab].rar` matches `c.rar` but not `a.rar` or `b.rar`.

---

## Executing Multiple Commands:

Often you may want to execute several commands together, either by running one 
after another or by passing output from one to another.

### Running Sequentially:

If you need to execute multiple commands in sequence but don't need to pass 
output between them, there are two options based on whether or not you want the 
subsequent commands to run only if the previous commands succeed or not. If you 
want the commands to run one after the other regardless of whether or not 
preceding commands succeed, place a `;` between the commands. 

For example, if you want to get information about your hardware, you could run 
```bash
lspci ; lsusb
```
which would output information on your PCI buses and USB devices in sequence.

However, if you need to conditionally run the commands based on whether the 
previous command has succeeded, insert `&&` between commands. 

An example of this is building a program from source, which is traditionally
done with `./configure`, `make`, and `make install`. The commands `make` and 
`make install` require that the previous commands have completed successfully, 
so you would use 
```bash
./configure && make && make install.
```

### Passing Output:

If you need to pass the output of one command so that it goes to the input of 
the next, after the character used between the commands, you need something 
called a `pipe`, which looks like a vertical bar or pipe `|`.

To use the `pipe`, insert the `|` between each command. 

For example, using the `|` in the command 
```bash
ls | less
``` 
allows you to view the contents of the ls more easily. 

---
