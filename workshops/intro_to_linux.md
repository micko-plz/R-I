# Into to Linux for Robotics

Don't be scared!

## Starting with VM

WORKSHOP #0
Make this its own workshop
Also explain the structure of future workshops
How to best communicate with us
Everything to get prepped for workshop #1
Helps to start getting feedback early
Put the linux motivation work into this workshop also -> presentation etc


## File and Folder layout on a Unix system

Lets examine the standard unix folder structure, from the top down. This structure is defined as a [standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) for Linux and unix-based operating systems.

Navigate to the top directory visually via clicking on the "Files" icon in the *favorites* toolbar on the left of the screen. Open *+ Other Locations* followed by *Computer*.
![test](root_folder.png)

The base folder is whats known as the system root directory. We'll now define the properties and contents of some of the subfolders in this directory.

| Name  | Properties                                                                         | Example                                                          |
| ----- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| /boot | Files needed to boot the system                                                    | linux kernel                                                     |
| /dev  | Device files (in unix systems everything is a file)                                | sda - first drive detected                                       |
| /etc  | System configuration files                                                         | hosts - IP's and addresses for local host etc                    |
| /home | User specific files and folders, one for each user                                 | *student's* (your) files and data such as code and study notes   |
| /opt  | Subfolders for optional packages, that don't have structure defined by the FHS      | All ROS-related shared libraries and executables                 |
| /proc | Files representing system and process info                                         | cpuinfo                                                          |
| /root | Personal files and folders for the root user, the super user / sysadmin            | similar to above                                                 |
| /usr  | Applications and libraries installed by any user. These are shared across by users | /usr/bin contains executables for basic OS use and bash commands |

## Terminal and the shell

Up till now we've navigated the system root directory visually via the file explorer. We'll now motivate the use of the command line for faster navigation, file creation and more.

Lets first open the terminal using the ```Ctrl+Alt+T```, you should see the following window pop up:

![test1](terminal.png)

The terminal is a graphical window that lets you interact with the *Shell*.

*Side note:*
We have installed *Terminator* and set it as the default terminal interface application. TODO: add terminator navigation tutorial.

The Shell is a textual (command line) interface to an operating systems applications and eventually the applications you will write. Every OS ships with a default shell; Powershell in Windows and the Bourne Again Shell (bash) on Linux/Mac platforms, which are similar in principal but have available to them different default commands for navigation and file manipulation.

Lets examine the text printed on the terminal: 
```student@robotics-irl:~$```
This is known as the *prompt*. This prompt tells you that you are user *student* on machine *robotics-irl*. Both of these were defined when we set up the VM. The *~* is shorthand for *home*, meaning you are currently in the home or base directory of user *student*. Finally, the *$* shows that you currently don't have root privileges, meaning commands that you run now will not be run as the superuser.

## Navigating and Examining Folders
Lets start running through a few commands

```bash
student@robotics-irl:~$ ls # Show the contents of the current working directory
student@robotics-irl:~$ ls -a # Above plus hidden folders or files (prefixed by .)
```

Feel free to confirm the terminal output with that of the File explorer (it default opens to the user's home directory)

After running ```ls -a``` you will notice two peculiar entries, that of *.* and *..*
With most (all?) Operating Systems, there exists the concept of absolute and relative paths. An absolute path to a file on your system is a */* separated list of the folders between said file and the *root* directory. Absolute paths are prefixed with */*. A relative path to a file is the path relative to where you currently are now. The directory above where we are now is referred to as *..*, our current directory is *.*

Side note: clarify what *home* directory usually refers to.

Lets put confirm this with some exercises. Knowing that we are in the home directory of the user *student* what do you think the absolute path of our current working directory is? Run the following to confirm this:

```bash
student@robotics-irl:~$ pwd # prints the absolute path of the current/present working directory
```
As you can see, we're in */home/student*, otherwise known as the *home* directory of user *student*. The above is an absolute path, so we can expect the folder *home* to be in the root directory, which we saw at the start of the tutorial.

In the terminal, we can jump to other directories via the *cd* (change directory) command. Confirm everything we mentioned above running the commands in the following script.

```bash
student@robotics-irl:~$ pwd
/home/student
student@robotics-irl:~$ cd .. # change into parent directory
student@robotics-irl:/home$ pwd
/home
student@robotics-irl:/home$ ls
student
student@robotics-irl:/home$ cd .. # change into parent directory
student@robotics-irl:/$ pwd # the root directory, also known as /
/
student@robotics-irl:/$ cd /home/student/ # change to your users home directory
student@robotics-irl:~$ cd / # go back to root directory
student@robotics-irl:/$ ls # show contents of root directory
bin   cdrom  etc   lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   tmp  var
student@robotics-irl:/$ cd ~ # same as "cd /home/student"
student@robotics-irl:~$ pwd
/home/student
```

TODO: move this
*Congratulations, you're now a hacker!


## Creating files and folders

```todo
mkdir, touch, cat
create a bash file, run it
how the shell interprets a file
shebang
ls -l
```

Now that we can navigate around the Ubuntu filesystem, lets start adding our own files to it.

```bash
student@robotics-irl:~$ cd ~ # make sure we are starting from the home directory
student@robotics-irl:~$ mkdir test_folder # create a folder called test_folder in the home directory (ie in the directory we are now in)
student@robotics-irl:~$ cd test_folder
student@robotics-irl:~/test_folder$ touch welcome # create a file called welcome in test_folder
student@robotics-irl:~/test_folder$ ls
welcome
student@robotics-irl:~/test_folder$ cat welcome # print the contents of this file (its empty for now) 
```

We first ensure that we are starting from the home directory. Remember, all paths are relative unless the name is prefixed with a */*. We then create a folder in this directory via the *mkdir* command. If we wanted to create this folder in the same location but from an alternative working directory, we could instead have given the full absolute path: ```mkdir /home/student/test_folder```. Within this folder we create a new and empty file via the *touch* command. The primary function of *touch* is to update the "accessed" timestamp of a file, but if the file name given does not already exist it will create a new and empty one. We can confirm this by running *cat* on the file which prints a file's contents to the terminal.

Lets start adding some content to the file. Open up the file using [Nano](https://help.ubuntu.com/community/Nano), a terminal-based text editor that ships with Ubuntu. Run ```nano welcome``` to open the file.

We're going to write a bash script that takes as argument a name, and prints it to the terminal along with the current date and time.

![test2](bash_script.png)

In the first line we evaluate the first argument passed to the script and assign its value to a variable called *name*. In the second line we run the  *date* command (the syntax $(function) calls a function and returns its output) and assign its output to variable *date_now*.

The last two lines print our desired output to the console, where the syntax ${var} evaluates the value of a variable with name *var*

Lets save this script via "CTRL+X", "Y", "ENTER".

The syntax we have used in this file is that of the bash language. So to run it as a bash script we enter the following in the terminal:

``` bash
bash welcome Atlas
```

While this is purely an academic exercises, scripting is super useful for automating repetitive and boring tasks. Oftentimes the bash language is suboptimal for some of these tasks and it may prove better to rely on a more expressive language like Python, or if speed is key then something like C. ([date](https://github.com/coreutils/coreutils/blob/master/src/date.c) is written in C). 

From a user's perspective, its shouldn't matter what language the script is written in, they should just be able to call it without having to specify how it is interpreted. We can tell the linux system that this file is a bash script and therefore should be interpreted as one by adding one additional line to the top of our script

``` bash
#!/bin/bash
```

This is whats known as a [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)). And tells the system to call *bash* from */usr/bin/bash*. We need now to let the system know that this script can be executed:

``` bash
chmod +x welcome
```

You can now run the scripts via 

```
./welcome Atlas
```

The same kind of functionality can just as easily be written in python via:

``` python
#!/bin/python3

import sys
from datetime import datetime

name = sys.argv[1]
date_now = datetime.now()

print("Welcome %s" % name)
print("Current time and date: %s" % date_now)
```

Save this file file also and try execute in the same way as above. You'll need to name it to something different to prevent a conflict with the bash script.

For scripts that we want to be able to execute from anywhere, without having to specify the full path (/home/student/test_folder/welcome Atlas), just like we can run *date* from anywhere, we need to add our script to the known path where Linux can search for executables.

This path is stored as global variable with name *PATH* and whose value can observed via:

``` bash
echo $PATH
#/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Using the *date* example again 



## Manipulating files and their paths 
mv, rm (rm -f), cp

## Install an executable
install an executable
$PATH, find a file





