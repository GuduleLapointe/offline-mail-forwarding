# Offline-Messages-mail-forwarding

Offline messages (IM) mail forwarding for OpenSimulator
https://github.com/GuduleLapointe/offline-mail-forwarding

GNU AFFERO GENERAL PUBLIC LICENSE (see LICENSE file)

# Warning

This forwarder might look easy to setup, but it is not an easy job.
Mail forwarding will expose your server. So you ABSOLUTELY need to have
a secured server to avoid being hacked or used by spammers.

Among other things:
* use SSL, disallow unencrypted access to this script
* if you have a mail relay installed on your server, deny relay from non authenticated users;
* avoid allowing group messages relaying if possible;
* keep watching your OpenSimulator, mail and web server logs to detect any abuse.

# Features

* Forward instant messages by mail when the user is offline
* Replacement for the usual offline message script, so it also handles local delivery

# Caveats

It is an external script. It is called where the sender stands by the region
simulator, because that's where you can call an external offline message script.

This means:
* if somebody sends a message from another grid, it will not be forwarded
because the other grid has no access to HG users email and IM preferences
* if it is sent from a region where the script it is not installed, the message
will not be forwarded either

This can be good, as it limits possible abuses, but a better approach would be
to intercept the message on the grid level (in Robust instance), and from
there allow or disallow forwarding from HG senders, but this would need
changes in OpenSimulator core software.

So this project is, and will stay, a workaround, not perfect but useful until
we or somebody else develop the core module.

# Installation

## On your webserver

You webserver needs access to the OpenSimulator database.

* copy the content in a folder called "offline" (or whatever you like) in your
webroot directory.
```bash
git clone https://github.com/GuduleLapointe/offline-mail-forwarding.git offline
```
* inside this directory, compy the example config file and adjust to your settings
```bash
cp config.php.example config.php
```

## In OpenSimulator (grid or standalone)

* In OpenSim.ini file adjust or add these setting under section [Messaging].
On a grid, this has to be set for every region.
```ini
[Messaging]
    OfflineMessageModule = OfflineMessageModule
    ; OfflineMessageModule = "Offline Message Module V2"
    OfflineMessageURL = http://your.server/offline/offline.php
    StorageProvider = OpenSim.Data.MySQL.dll
    MuteListModule = MuteListModule
    MuteListURL = ${Const|BackOffice}/mute.php
    ForwardOfflineGroupMessages = false
```
