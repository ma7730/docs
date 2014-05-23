Spark CLI
==========

The Spark CLI is a powerful tool for interacting with your cores and the Spark Cloud.  The CLI uses [node.js](http://nodejs.org/) and can run on Windows, Mac OS X, and Linux fairly easily.  It's also [open source](https://github.com/spark/spark-cli) so you can edit and change it, and even send in your changes as [pull requests](https://help.github.com/articles/using-pull-requests) if you want to share!

Installing
=======

  First, make sure you have [node.js](http://nodejs.org/) installed!  

  Next, open a command prompt or terminal, and install by typing:

```sh 
$ npm install -g spark-cli
$ spark cloud login
```

Install (advanced)
---------------------------

To use the local flash and key features you'll need to install [dfu-util](http://dfu-util.gnumonks.org/), and [openssl](http://www.openssl.org/).  They are freely available and open-source, and there are installers and binaries for most major platforms as well.  

Here are some great tutorials on the community for full installs:

[Installing on Ubuntu](https://community.spark.io/t/how-to-install-spark-cli-on-ubuntu-12-04/3474)

[Installing on Windows](https://community.spark.io/t/tutorial-spark-cli-on-windows-06-may-2014/3112)


Upgrading
---------------------------
To upgrade Spark-CLI, enter the following command:

```sh
$ npm update -g spark-cli
```


Running from source (advanced)
---------------------------
To grab the CLI source and play with it locally

    git clone git@github.com:spark/spark-cli.git
    cd spark-cli/js
    node app.js help






Getting Started
===============

  These next two commands are all you need to get started setting up an account, claiming a core, and discovering new features.


###spark setup

  Guides you through creating a new account, and claiming your core!

``` $ spark setup```


###spark help

  Shows you what commands are available, and how to use them.  You can also give the name of a command for detailed help.
  
```sh
$ spark help
$ spark help keys 
```




Command Reference
================

###spark setup wifi

  Helpful shortcut for adding another wifi network to a core connected over USB.  Make sure your core is connected via a USB cable, and is slow blinking blue [listening mode](http://docs.spark.io/#/connect)

``` $ spark setup wifi```


###spark login

  Login and save an access token for interacting with your account on the Spark Cloud.

``` $ spark login ```


###spark logout

  Logout and optionally revoke the access token for your CLI session.

``` $ spark logout ```


###spark list

Generates a list of what cores you own, and displays information about their status, including what variables and functions are available

```sh
$ spark list

Checking with the cloud...
Retrieving cores... (this might take a few seconds)
my_core_name (0123456789ABCDEFGHI) 0 variables, and 4 functions
  Functions:
    int digitalWrite(string)
    int digitalRead(string)
    int analogWrite(string)
    int analogRead(string)

```

###spark core add

  Adds a new core to your account

```sh 
$ spark cloud claim 0123456789ABCDEFGHI
Claiming core 0123456789ABCDEFGHI
Successfully claimed core 0123456789ABCDEFGHI
```


###spark core rename

  Assigns a new name to a core you've claimed

```$ spark core rename 0123456789ABCDEFGHI "pirate frosting" ```



###spark core remove

  Removes a core from your account so someone else can claim it.

```sh
$ node app.js core remove 0123456789ABCDEFGHI
Are you sure?  Please Type yes to continue: yes
releasing core 0123456789ABCDEFGHI
server said  { ok: true }
Okay!
```


###spark flash

  Sends a firmware binary, a source file, or a directory of source files, or a known app to your core.

####Flashing a directory

  You can setup a directory of source files and libraries for your project, and the CLI will use those when compiling remotely.  You can also create ```spark.include``` and / or a ```spark.ignore``` file in that directory that will tell the CLI specifically which files to use or ignore.

```sh
$ spark flash 0123456789ABCDEFGHI my_project
```

####Flashing one or more source files

```sh
$ spark flash 0123456789ABCDEFGHI app.ino library1.cpp library1.h
```

####Flashing a known app

```sh
$ spark flash 0123456789ABCDEFGHI tinker
$ spark flash 0123456789ABCDEFGHI cc3000
```

####Compiling remotely and Flashing locally

To work locally, but use the cloud compiler, simply use the compile command, and then the local flash command after.  Make sure you connect your core via USB and place it into [dfu mode](http://docs.spark.io/#/connect/appendix-dfu-mode-device-firmware-upgrade).

```sh
$ spark compile my_project_folder --saveTo firmware.bin
OR
$ spark compile app.ino library1.cpp library1.h --saveTo firmware.bin
$ spark flash --usb firmware.bin
```


###spark compile

  Compiles one or more source file, or a directory of source files, and downloads a firmware binary.

####compiling a directory

  You can setup a directory of source files and libraries for your project, and the CLI will use those when compiling remotely.  You can also create ```spark.include``` and / or a ```spark.ignore``` file in that directory that will tell the CLI specifically which files to use or ignore.  Those files are just plain text with one line per filename

```sh
$ spark compile my_project_folder
```

####example spark.include
```text
application.cpp
library1.h
library1.cpp
```

####example spark.ignore
```text
.ds_store
logo.png
old_version.cpp
```


####Flashing one or more source files

```sh
$ spark compile app.ino library1.cpp library1.h
```




###spark call

  Calls a function on one of your cores, use ```spark list``` to see which cores are online, and what functions are available.

    $ spark call 0123456789ABCDEFGHI digitalWrite "D7,HIGH"
    1



###spark get

  Retrieves a variable value from one of your cores, use ```spark list``` to see which cores are online, and what variables are available.

    $ spark get 0123456789ABCDEFGHI temperature
    72.1



###spark monitor

  Pulls the value of a variable at a set interval, and optionally display a timestamp
  
  * Minimum delay for now is 500 (there is a check anyway if you keyed anything less)
  * hitting ```CTRL + C``` in the console will exit the monitoring

```sh
$ spark monitor 0123456789ABCDEFGHI temperature 5000
$ spark monitor 0123456789ABCDEFGHI temperature 5000 --time
$ spark monitor all temperature 5000
$ spark monitor all temperature 5000 --time
$ spark monitor all temperature 5000 --time > my_temperatures.csv
```


###spark identify

  Retrieves your core id when the core is connected via USB and in listening mode (flashing blue).

```sh
$ spark identify
$ spark identify 1
$ spark identify COM3
$ spark identify /dev/cu.usbmodem12345

$ spark identify
0123456789ABCDEFGHI
```

###spark subscribe

  Subscribes to published events on the cloud, and pipes them to the console.  Special core name "mine" will subscribe to events from just your cores.


```sh 
$ spark subscribe
$ spark subscribe mine
$ spark subscribe eventName
$ spark subscribe eventName mine
$ spark subscribe eventName CoreName
$ spark subscribe eventName 0123456789ABCDEFGHI
```




###spark serial list

  Shows currently connected Spark Core's acting as serial devices over USB

``` $ spark serial list ```


###spark serial monitor

  Starts listening to the specified serial device, and echoes to the terminal

```sh
$ spark serial monitor
$ spark serial monitor 1
$ spark serial monitor COM3
$ spark serial monitor /dev/cu.usbmodem12345
```


###spark keys doctor

Helps you update your keys, or recover your core when the keys on the server are out of sync with the keys on your core.  The ```spark keys``` tools requires both dfu-util, and openssl to be installed.

Connect your core in [dfu mode](http://docs.spark.io/#/connect/appendix-dfu-mode-device-firmware-upgrade), and run this command to replace the unique cryptographic keys on your core.  Automatically attempts to send the new public key to the cloud as well.

``` $ spark keys doctor 0123456789ABCDEFGHI```


###spark keys new

Generates a new public / private keypair that can be used on a core.

```sh
$ spark keys new
running openssl genrsa -out core.pem 1024
running openssl rsa -in core.pem -pubout -out core.pub.pem
running openssl rsa -in core.pem -outform DER -out core.der
New Key Created!

$ spark keys new mykey
running openssl genrsa -out mykey.pem 1024
running openssl rsa -in mykey.pem -pubout -out mykey.pub.pem
running openssl rsa -in mykey.pem -outform DER -out mykey.der
New Key Created!
```

###spark keys load

Copies a ```.DER``` formatted private key onto your core's external flash.  Make sure your core is connected and in [dfu mode](http://docs.spark.io/#/connect/appendix-dfu-mode-device-firmware-upgrade).  The ```spark keys``` tools requires both dfu-util, and openssl to be installed.  Make sure any key you load is sent to the cloud with ```spark keys send core.pub.pem```

```sh
$ spark keys load core.der
...
Saved!
```

###spark keys save

Copies a ```.DER``` formatted private key from your core's external flash to your computer.  Make sure your core is connected and in [dfu mode](http://docs.spark.io/#/connect/appendix-dfu-mode-device-firmware-upgrade).  The ```spark keys``` tools requires both dfu-util, and openssl to be installed.

```sh
$ spark keys save core.der
...
Saved!
```

###spark keys send

Sends a core's public key to the cloud for use in opening an encrypted session with your core.  Please make sure your core has the corresponding private key loaded using the ```spark keys load``` command.

```sh
$ spark keys send 0123456789ABCDEFGHI core.pub.pem
submitting public key succeeded!
```

###spark keys server

Switches the server public key stored on the core's external flash.  This command is important when changing which server your core is connecting to, and the server public key helps protect your connection.   Your core will stay in DFU mode after this command, so that you can load new firmware to connect to your server.

Coming Soon - more commands to make it easier to change the server settings on your core!


```sh
$ spark keys server my_server.der
Okay!  New keys in place, your core will not restart.
```
