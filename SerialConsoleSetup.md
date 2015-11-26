

# Introduction #

The mini2440 provides a serial port for console output. As such, a terminal emulator is required to communicate with the board.

**Serial Port Configuration 115k (8/N/1)**
  * BAUD-Rate: 115 kbps
  * Flow-Control: None
  * Parity: None
  * Size: 8 bits

# Terminal Emulators #

While Windows has polished programs like Hyperterm and Teraterm for terminal emulation, linux terminal emulation is typically done from the command line (terminal).

As such, several different options exist for terminal emulators in a linux environment:
  * [Minicom](http://freshmeat.net/projects/minicom/)
  * [Picocom](http://code.google.com/p/picocom/)
  * [UUCP](http://www.airs.com/ian/uucp.html)

For a detailed explanation please review [Google Books: Building Embedded Systems > Terminal Emulators](http://books.google.com/books?id=xnFdWfJAK9wC&lpg=PA150&ots=GESIO5ZZ3Y&dq=cu%20terminal%20emulator&pg=PA150#v=onepage&q=terminal%20emulators&f=false)

# Picocom #

As done [here](http://code.google.com/p/mini2440/wiki/MiniBringup), the Picocom terminal emulator will be used.

_From the Picocom project page:_
> As its name suggests, picocom is a minimal dumb-terminal emulation program. It is, in   principle, very much like minicom, only it's pico instead of mini!

> Picocom was designed to serve as a simple, manual, modem configuration, testing, and debugging tool. It has also served (quite well) as a low-tech "terminal-window" to allow operator intervention in PPP connection scripts (something like the ms-windows "open terminal window before / after dialing" feature). It could also prove useful in many other similar tasks.
## Installing Picocom ##

While you can build Picocom from [source](http://picocom.googlecode.com/files/picocom-1.4.tar.gz) it is recommended that you install it from your linux distributions package manager (e.g. Yum, Apt, etc.). From a terminal type one of the following commands corresponding with your particular system:
  * sudo yum install picocom
  * sudo apt-get install picocom
  * sudo emerge picocom

If you are not on the sudoers list and get a nasty message go [here](http://www.pendrivelinux.com/how-to-add-a-user-to-the-sudoers-list/) and try again or become root via 'su'.

Alternatively, you may also use the GUI package management front-ends (e.g. synaptic for apt-get or yumex for yum) if the command-line is too scary. Just search for the 'picocom' package.

## Using Picocom ##

The first serial port on a PC (COM1) is typically assigned to /dev/ttyS0. Similarly, when using a USB-to-Serial adapter the port is typically at /dev/ttyUSB0.

To connect to the mini2440 via COM1:

```
someone@something ~ $ sudo picocom -b 115200 /dev/ttyS0 --send-cmd "sx -vv"
```


Note: It **may not** be necessary to execute the above with elevated privileges depending on your platform.

### Commands ###

All the commands for Picocom can be viewed via 'man picocom'. However, some common commands are listed:

**Escape Character `<ctrl-a>`:**
```
          Send the escape character to the serial port and return  to  "trans‐
          parent"  mode.  This  means  that if the escape character ("C-a", by
          default) is typed twice, the program sends the escape  character  to
          the  serial  port,  and  remains  in transparent mode. This is a new
          behavior implemented in v1.4. Previously picocom used to ignore  the
          escape-character when it was entered as a function character.
```
**Exit `<ctrl-a><ctrl-x>`:**
```
          Exit  the  program: if the "--noreset" option was not given then the
          serial port is reset to its original settings before exiting; if  it
          was given the serial port is not reset.
```
**NOTE:**It is important to exit the terminal session every time it is opened from Picocom. Failure to do so can lock the /dev/ttyS0 device. Sometimes it can be challenging to unlock.

**Send File `<ctrl-a><ctrl-s>`:**
```
          Send (upload) a file using modem protocol specified during session creation (e.g. xmodem or zmodem) 
```







## Customization (OPTIONAL) ##
As a matter of convenience, the picocom session command can be put into a bash script and saved somewhere convenient (e.g. /home/user/bin).

To do this, open a text editor such as gedit or vim and type the following:
```
#!/bin/bash
exec su -c 'picocom -b 115200 /dev/ttyS0 --send-cmd "sx -vv"'
```

Save the file as 'miniconsole' to '/home/$USER/bin' where $USER is your user name. By default the file will not be executable. As such, do the following:
```
someone@something ~ $ cd /home/someone/bin
someone@something ~/bin $ chmod +x miniconsole
```

The above can be verified by doing:
```
someone@something ~ $ ls -l `which miniconsole`
-rwxr-xr-x 1 someone users 75 2009-09-06 12:40 /home/someone/bin/miniconsole
```
If miniconsole is not found then its location isn't in your $PATH. To temporarily fix this the directory can be appended to your $PATH via:
```
someone@something ~ $ export PATH=$PATH:/home/someone/bin
```
However, an entry should be put in your .bashrc file to ensure that the directory is always added to your $PATH when you login. Go [here](http://tldp.org/LDP/abs/html/sample-bashrc.html) for more information regarding the .bashrc file.