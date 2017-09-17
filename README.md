# Quake3 Arena CPMA Dedicated Server Guide Linux
Challenge ProMode Arena (CPMA, formerly Challenge ProMode [CPM], unofficially Promode) is a freeware modification for id Software's first-person shooter computer game Quake III Arena (Q3A). CPMA includes modified gameplays that feature air-control, rebalanced weapons, instant weapon switching and additional jumping techniques.

__[Official website](http://playmorepromode.org/) - 
[News site](https://www.plusforward.net/cpma/) - 
[Subreddit](https://www.reddit.com/r/CPMA/) - 
[Discord](https://discordapp.com/invite/GHRUqR)__

<br />
<br />
<br />
<br />

This tutorial will guide you on how to host a/multiple Quake 3 Arena CPMA dedicated servers(s) on linux.

## Table of contents :

* [Requirements](#Requirements)
* [Installation 1/2 (as root)](#Installation1/2)
* [Installation 2/2 (as quake3)](#Installation2/2)
* [Managing the server(s)](#Managing)
* [Automatic server restart](#Restart)

<br />
<br />
<br />
<br />

## <a name="Requirements"></a>Requirements
* Debian 9
* 1 CPU and 1GB of RAM available on your server.
* I recommend using the $2.50 or $5 offer from [Vultr](https://www.vultr.com/pricing/) (not affiliated with them, it's just what I currently use)

<br />
<br />
<br />

# <a name="Installation1/2"></a>Installation 1/2 (as root) :
```
su
```
## Install dependencies
### system
```
dpkg --add-architecture i386 && apt-get update -y
```

### screen, zip and unzip
```
apt-get install -y screen zip unzip
```
### Add the quake3 user (if necessary)
The Quake 3 Arena CPMA server files will be installed in the "quake3" user home directory, server instances will be launched as the quake3 user.

Skip this part if you already have a quake3 user on your server.

### Add the user
```
useradd quake3 -m -r -s /bin/bash
```

### Change the quake3 user password (optional, recommended)
```
echo "quake3:yourquake3password" | chpasswd
```

(If you plan on logging as quake3 in a ssh session, don't forget to allow ssh password authentication for non root users in your ssh config file).

<br />
<br />
<br />

# <a name="Installation2/2"></a>Installation 2/2 (as quake3) :
```
su quake3
``` 
And
```
script /dev/null
```
Or from another machine :

```ssh quake3@your_server_ip``` (or use PuTTY)
## File installation
### Download and extract Quake3 and CPMA
```
cd ~ && mkdir quake3_arena/ && wget -O q3cpma.zip "https://downloader.disk.yandex.ru/disk/7185619c656cf1c36b6ac96d565b49c10491ade90b642e3f04189774e25dc0ec/59beffb4/53sDzpfHuBppuItITrHhDSXttgRq58GxhpUYtvwuBn1Q4cETy9RuB7C8QK3r_lYYJoi6yiCYYohEcJoSe17vaw%3D%3D?uid=0&filename=CPMA_full_rc4.zip&disposition=attachment&hash=YYXD3S8CqzHfF6LaZ3/izXVmV5myMZopfIfUbmvlbw8%3D%3A&limit=0&content_type=application%2Fx-zip-compressed&fsize=1257218978&hid=390c29783673c668af5f5654f594af07&media_type=compressed&tknv=v2"  && unzip -d quake3_arena q3cpma.zip  && cd quake3_arena && rm *.txt && rm h4* && rm -r superhudeditor-0.3.0 udt_gui_0.7.2_dll_1.3.0 docs
```

The yandex link used below is a direct link to the [DEZ's Q3CPMA archive](https://drive.google.com/open?id=0B0wfFzzjvCheTVVzSmdnQ1BLUms)

### Download and extract Quake3e
```
cd /home/quake3/quake3_arena/ && wget http://www.edawn-mod.org/binaries/quake3-1.32e.zip && unzip quake3-1.32e.zip && chmod +x quake3e*
```

"Quake3e is an improved client for playing Quake 3 Arena mods" [Official site](http://edawn-mod.org/forum/viewtopic.php?f=6&t=7)

### Download a default server config
```
wget -O /home/quake3/quake3_arena/baseq3/server.cfg https://www.dropbox.com/s/syohxlyr8da2hhm/default_cpma_server.cfg
```

This cfg file will be loaded when a server instance is launched later on, it contains good default parameters. 

Some of these parameters should however be overwritten later on in the launch command.

Your Quake 3 Arena CPMA server files are now installed, server instance(s) can be launched !


### Edit server.cfg (optional, not recommended)
```
nano /home/quake3/quake3_arena/baseq3/server.cfg
```  

(Ctrl-O to save, Ctrl-X to exit the editor)

This file contains the server settings that will be applied when you launch the server. I recommend reading the file and understanding the commands.
```
more /home/quake3/quake3_arena/baseq3/server.cfg
```

I don't recommend modifying this file as it will be executed at server launch (this is optional though) 

(You can however duplicate it and have your custom settings if you plan on not passing any start parameters but executing a custom config)

It's best to leave this file untouched and use start parameters after the launch command (eg : +sv_hostname m3fh4q Q3_CPMA server)

<br />
<br />
<br />

Your Quake 3 Arena CPMA server files are now installed, server instance(s) can be launched !

# <a name="Managing"></a>Managing the server(s)
The server(s) will be fully managed with the quake3 user, __log in as quake3 for this section__
```
su quake3
``` 
And
```
script /dev/null
```
## Operations
### Connect to the server(s)
You can connect to your server using the following command (in console)

```
connect yourserverip:port_of_the_instance
``` 

### Create screen session(s)
```
screen -dmS cpma_server1
``` 

This will create a detached terminal called "cpma_server1"

Each instance of a Quake 3 CPMA dedicated server needs to be launched in a detached terminal using [screen](https://www.gnu.org/software/screen/manual/screen.html), if you launch a server from your current session, it will shutdown when you exit it. 

Open as many screen sessions as Quake 3 Arena CPMA dedicated server instances you intend to host on this server, a 1 CPU and 1GB of RAM server can usually handle 2 instances with sv_maxclients 8.

### Prepare your server(s) launch settings string
Prepare a string of settings that will follow the launch command, use all the necessary sv_ commands

Example of a string of settings : 

>__+set dedicated 2 +set fs_game cpma__ +exec server.cfg +sv_hostname m3fh4q Q3CPMA server +set net_port 27960"

"+set dedicated 2 +set fs_game cpma" is mandatory !

The most important command is set net_port, each Quake 3 Arena CPMA server instance on your server needs to have a different one, sample : 27960 and 27961 if you have 2 servers, 27962 afterwards etc...

Create your own string of settings and save it somewhere or create a .cfg file in the /home/quake3/quake3_arena/baseq3/ directory.

### Start the server(s)
The line break with " is important !
```
screen -S screen_session_name -X stuff "cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 launch_setting_string
"
```

Using the example in this guide :

```
screen -S cpma_server1 -X stuff "cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 +set dedicated 2 +set fs_game cpma +exec server.cfg +sv_hostname m3fh4q Q3CPMA server +set net_port 27960
"
```

Generic :
```
screen -S screen_session_name -X stuff "cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 +set dedicated 2 +set fs_game cpma
"
```

__Or (if you're using a seperate custom server cfg file)__

```
screen -S cpma_server1 -X stuff "cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 +set dedicated 2 +set fs_game cpma +exec custom_server_cfg.cfg
"
```

### Shutdown the server(s)
Using the example in this guide :
```
screen -S cpma_server1 -X stuff "quit
"
```

Generic :
```
screen -S screen_session_name -X stuff "quit
"
```

### Using the server console
To use the server console, you need to enter the screen session associated with it.

Press Ctrl+A and Ctrl+D at the same time to detach from session 

Using the example in this guide :
```
screen -r cpma_server1
```

Generic :
```
screen -r screen_session_name
``` 

### Update the server(s)
If the CPMA mod gets an update (Last update was July 27th 2010)

* Shutdown the Quake 3 Arena CPMA dedicated server instance(s) running on your server (Stop the server(s)) using the instructions above.

* Remove the /home/quake3/quake3_arena/cpma directory

* Put the new cpma directory in /home/quake3/quake3_arena/

* You will probably have to update Quake3e, in that case, remove all the Quake3e* files in /home/quake3/quake3_arena/ , download and extract the new Quake3e client files in /home/quake3/quake3_arena/ , [Quake3e official site](http://edawn-mod.org/forum/viewtopic.php?f=6&t=7)

* Restart the servers

<br />
<br />
<br />

# <a name="Restart"></a>Automatic server restart
After some time, Quake 3 Arena CPMA dedicated servers can feel unresponsive, which is why they need to be restarted every 24 hours.

To do this, we'll use the cron utility to perform a sever shutdown and restart.

### Creating the strings for cron
Shutdown (using the example in this guide) :
>00 06 * * * screen -S cpma_server1 -X stuff "quit\r"

Start (using the example in this guide) :
>01 06 * * * screen -S cpm_server1 -X stuff ""cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 +set dedicated 2 +set fs_game cpma +exec server.cfg +sv_hostname m3fh4q Q3CPMA server +set net_port 27960\r"

__Create your strings with your own server settings you used earlier__

### Adding the strings as cron entries
You can edit the crontab file using :
```
crontab -e
```
If you're using nano : Ctrl-O to save, Ctrl-X to exit.

__add both strings previously created as a single line each__

You can list the cron entries using :
```
crontab -l
```

Using the example in this guide, ```crontab -l``` will return this :
>#a bunch of comments

>00 06 * * * screen -S cpma_server1 -X stuff "quit\r"

>01 06 * * * screen -S cpm_server1 -X stuff ""cd /home/quake3/quake3_arena/ && ./quake3e.ded.x64 +set dedicated 2 +set fs_game cpma +exec server.cfg +sv_hostname m3fh4q Q3CPMA server +set net_port 27960\r"

In this example, The Quake 3 Arena CPMA dedicated server instance running in the screen session called cpma_server1 will be shutdown at 06:00 and restarted at 06:01 system time (generally UTC)