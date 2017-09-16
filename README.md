Challenge ProMode Arena (CPMA, formerly Challenge ProMode [CPM], unofficially Promode) is a freeware modification for id Software's first-person shooter computer game Quake III Arena (Q3A). CPMA includes modified gameplays that feature air-control, rebalanced weapons, instant weapon switching and additional jumping techniques.

__[Official website](http://playmorepromode.org/) - 
[News site](https://www.plusforward.net/cpma/) - 
[Subreddit](https://www.reddit.com/r/CPMA/) - 
[Discord](https://discordapp.com/invite/GHRUqR)__

<br />
<br />
<br />
<br />

This tutorial will guide you on how to host a/multiple Quake 3 Arena CPMA servers(s) on linux.

## Table of contents :

* [Requirements](#Requirements)
* [Installation 1/2 (as root)](#Installation1/2)
* [Installation 2/2 (as steam)](#Installation2/2)

## Requirements <a name="Requirements"></a>
* Debian 9
* 1 CPU and 512Mb of RAM available on your server.
* I recommend using the $2.50 or $5 offer from [Vultr](https://www.vultr.com/pricing/) (not affiliated with them, it's just what I currently use)

# Installation 1/2 (as root) : <a name="Installation1/2"></a>
```
su
```
## Install dependencies
### system
```
dpkg --add-architecture i386
```
```
apt-get update -y
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

# Installation 2/2 (as quake3) : <a name="Installation2/2"></a>
## File installation
### Download and extract latest quake3e
"Quake3e is an improved client for playing Quake 3 Arena mods" [Official site](http://edawn-mod.org/forum/viewtopic.php?f=6&t=7)

```
cd ~ && wget http://www.edawn-mod.org/binaries/quake3-1.32e.zip
```
```
unzip quake3-1.32e.zip
```