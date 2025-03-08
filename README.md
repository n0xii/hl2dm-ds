# **Half-Life 2: Deathmatch dedicated server guide.**

This guide will explain how to host a dedicated server for Half-Life 2: Deathmatch on the following platforms:
- [Windows](#Windows)
- [Linux](#Linux)
- [Docker](#Docker)

*Docker is, in my opinion, the easiest way to host and manage your server for long term use, docker doesn't use a lot of resources, is easy to interact with and the server is easy to manage.*

**Following this, the guide will explain how to [configure](#configuration) your server, configure [FastDL](https://developer.valvesoftware.com/wiki/FastDL) and install addons such as [Metamod](https://www.metamodsource.net/) and [Sourcemod](https://www.sourcemod.net/) with [plugins](#plugins).**

Post installation or if you already have the server installed, use the following links to configure your server further. 
- [Configuration](#configuration)
- [FastDL](#fastdl)
- [Addons](#addons)
	- [Metamod](https://www.metamodsource.net/)
	- [Sourcemod](https://www.sourcemod.net/)
- [Plugins](#plugins)

## **Windows**

### **Step 1: Setting up your folder structure.**
For the installation of the server we're going to need a folder for [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD) and a folder for the server itself, take the following as an example:
```
Half-Life 2: Deathmatch Dedicated Server/
│
├── steamcmd/
│
└── server/
```
In this example, the Half-Life 2: Deathmatch dedicated server will be installed into the `server` folder and steamCMD, which is used to download the server, will be installed into `steamcmd`. It is up to you where you want to place these folders. 

### **Step 2: Downloading SteamCMD.**
The next step is to download SteamCMD, a download link for Windows can be found [here](https://developer.valvesoftware.com/wiki/SteamCMD#Windows), **make sure you download the one under Windows, the file is called steamcmd.zip .**
Afterwards, unzip this file and put all of its content in the `server` folder, it should now look like this:
```
Half-Life 2: Deathmatch Dedicated Server/
│
├── steamcmd/
│   └── steamcmd.exe
│
└── server/
```

### **Step 3: Running SteamCMD.**
After downloading, we can double click on steamcmd.exe to run it, following this, a terminal window will open which returns the following output:

```
[  0%] Checking for available updates...
[----] Verifying installation...
...
```
After it has downloaded updates, checked for updates and verified the installation we can continue.

### **Step 4: SteamCMD setting installation path.**
Before logging into SteamCMD we must first set the location for the dedicated server to be downloaded and installed to, note that for this we have to use the absolute path.

`force_install_dir path\to\your\server\`

For example, installing the server onto your D drive would be: 

`force_install_dir D:\Half-Life 2: Deathmatch Dedicated Server\server\`

When setting this on Windows make sure to use the back slash `\` instead of the forward slash `/`.

### **Step 5: Logging in anonymously.**
The next step is to run steamcmd.exe by double clicking on it, this should launch a terminal window running SteamCMD. SteamCMD should update itself if necessary and it should end up with a prompt in which you can type commands. 
We can use the anonymous login feature to download the dedicated server as we don't need to make a purchase to download it, the command to login anonymously is:

`login anonymous`

Following this we should be logged into SteamCMD anonymously, SteamCMD will tell us: 
```
Connecting anonymously to Steam Public...OK
Waiting for client config...OK.
Waiting for user info...OK
```
### **Step 6: Downloading the dedicated server.**
Now that we are logged in and the installation directory is set we can download the dedicated server, to do this we use the following command:

`app_update 232370 validate`

`app_update` will download appid `232370` (Half-Life 2: Deathmatch Dedicated Server) and `validate` the files after downloading.
When the download is complete you may close SteamCMD and your server directory should be populated with files like so:
```
Half-Life 2: Deathmatch Dedicated Server/
│
├── steamcmd/
│   ├── steamcmd.exe
│   └── ...
│
└── server/
    ├── hl2mp/
    │   ├── ...
    │   └── ...
    ├── srcds.exe
    └── ...
```
We can see that SteamCMD is downloading our server as the terminal writes the following output:
```
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x61) downloading, progress: 0.00 (0 / 1049938646)
 Update state (0x61) downloading, progress: 4.09 (42987117 / 1049938646)
 Update state (0x61) downloading, progress: 6.80 (71391309 / 1049938646)
 Update state (0x61) downloading, progress: 12.91 (135573772 / 1049938646)
 Update state (0x61) downloading, progress: 19.76 (207506150 / 1049938646)
 Update state (0x61) downloading, progress: 28.76 (301982332 / 1049938646)
...
```
Note: the progress is shown here in % and bytes. Following the download, it will validate the files which looks like this:
```
 Update state (0x81) verifying update, progress: 2.56 (26908998 / 1049938646)
 Update state (0x81) verifying update, progress: 9.25 (97137605 / 1049938646)
 Update state (0x81) verifying update, progress: 15.24 (160053481 / 1049938646)
 Update state (0x81) verifying update, progress: 21.81 (229023692 / 1049938646)
 Update state (0x81) verifying update, progress: 26.41 (277301554 / 1049938646)
 Update state (0x81) verifying update, progress: 31.65 (332265526 / 1049938646)
 Update state (0x81) verifying update, progress: 38.77 (407038805 / 1049938646)
 Update state (0x81) verifying update, progress: 44.93 (471691466 / 1049938646)
...
```
When finished, it will tell us `Success! App '232370' fully installed.`

### **Step 7: Running the dedicated server**
Inside the server directory there should be a file called srcds.exe which will be used to run the server, to do this we make a .bat file to easily run the server and set parameters. The easiest way to do this is to create text file, enter the following command in this file and then save it as .bat instead of a .txt file.  
```
srcds.exe -console -game hl2mp -maxplayers 10 +map dm_lockdown
```
Optionally you can omit `-console` to run the server using a graphical interface. The `-exec` parameter is used to execute a .cfg (configuration) file, in this case we execute server.cfg which contains the settings for the server such as the hostname and the password. Running the bat file will start the server, we can see that the server is started when the server window tells us:
```
Connection to Steam servers successful.
   Public IP to Steam is xxx.xxx.xxx.xxx
Assigned anonymous gameserver Steam ID [A:x:xxxxxxxxxx:xxxxx].
VAC secure mode is activated.
```
**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**

## Linux

### **Step 1: installing prerequisites.**
Modern versions of Linux allow you to install SteamCMD directtly from your package manager, for example on my test setup ruinning Pop OS, i can use the command:
```
sudo apt install steamcmd
```
Which will return:
```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libstdc++6:i386
Suggested packages:
  steam:i386
The following NEW packages will be installed:
  libstdc++6:i386 steamcmd:i386
0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,125 kB of archives.
After this operation, 7,191 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```
After this we can type y and press enter to start the installation. If your package manager and or package repository does not include SteamCMD you might have to manually install the dependencies and manually download the package, how this is done is described [here](https://developer.valvesoftware.com/wiki/SteamCMD#Linux).

### **Step 2: Creating a user for SteamCMD.**
Running the server under the `root` user is a *bad idea* and a *scurity risk*, most distributions force you to make a user account which should be used to run the server instead, if you do not have a user account, make one like so:
```
sudo useradd -m newusername
sudo passwd newusername
```

### **Step 3: Setting up your folder structure.**
Since we've downloaded SteamCMD (if possible) from the repositories we will not need to make a folder for SteamCMD. We will still need to make a folder for the server to be installed to, take the following as an example:
```
/home/username/hl2dm-ds/
                        │
                        └── server/
```
In this example, the Half-Life 2: Deathmatch dedicated server will be installed into the `server` folder. To create the new folders open a new terminal and type the following commands:
```
cd ~
mkdir hl2dm-ds
cd hl2dm-ds
mkdir server
```

### **Step 4: Running SteamCMD.**
After installing SteamCMD and sertting up the folder structure, we can run SteamCMD with the following command:
`steamcmd`
Following this the prompt should return something like this:
```
[  0%] Checking for available updates...
[----] Verifying installation...
...
```
After it has downloaded updates, checked for updates and verified the installation we can continue.

### **Step 5: Setting your installation directory.**
Before logging into SteamCMD we must first set the location for the dedicated server to be downloaded and installed to, note that for this we have to use the absolute path.

`force_install_dir /path/to/your/server`

For example, installing the server into your home folder (recommended, permission will be set accordingly) 

`force_install_dir /home/username/hl2dm-ds/server`

When setting this on Linux make sure to use the forward slash `/` instead of the back slash `\`.

### **Step 6: Logging in anonymously.**
The next step is to login to SteamCMD using an anonymous account as we do not need to purchase HL2:DM or anything to be able to download the server, to login, use the following command:

`login anonymous`

Following this we should be logged into SteamCMD anonymously, SteamCMD will tell us: 
```
Connecting anonymously to Steam Public...OK
Waiting for client config...OK.
Waiting for user info...OK
```

### **Step 7: Downloading the dedicated server.**
Now that we are logged in and the installation directory is set we can download the dedicated server, to do this we use the following command:

`app_update 232370 validate`

`app_update` will download appid `232370` (Half-Life 2: Deathmatch Dedicated Server) and `validate` the files after downloading.
When the download is complete you may close SteamCMD and your server directory should be populated with files like so:
```
/home/username/hl2dm-ds/server/
                               ├── hl2mp/
                               │
                               └── srcds_run
```
We can see that SteamCMD is downloading our server as the terminal writes the following output:
```
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x61) downloading, progress: 0.00 (0 / 1049938646)
 Update state (0x61) downloading, progress: 4.09 (42987117 / 1049938646)
 Update state (0x61) downloading, progress: 6.80 (71391309 / 1049938646)
 Update state (0x61) downloading, progress: 12.91 (135573772 / 1049938646)
 Update state (0x61) downloading, progress: 19.76 (207506150 / 1049938646)
 Update state (0x61) downloading, progress: 28.76 (301982332 / 1049938646)
 ...
```
Note: the progress is shown here in % and bytes. Following the download, it will validate the files which looks like this:
```
 Update state (0x81) verifying update, progress: 2.56 (26908998 / 1049938646)
 Update state (0x81) verifying update, progress: 9.25 (97137605 / 1049938646)
 Update state (0x81) verifying update, progress: 15.24 (160053481 / 1049938646)
 Update state (0x81) verifying update, progress: 21.81 (229023692 / 1049938646)
 Update state (0x81) verifying update, progress: 26.41 (277301554 / 1049938646)
 Update state (0x81) verifying update, progress: 31.65 (332265526 / 1049938646)
 Update state (0x81) verifying update, progress: 38.77 (407038805 / 1049938646)
 Update state (0x81) verifying update, progress: 44.93 (471691466 / 1049938646)
 ...
```
When finished, it will tell us `Success! App '232370' fully installed.`. To close SteamCMD we can use the command `exit`.

### **Step 8: Running the dedicated server**
Inside the server directory there should be a file called srcds_run which we will use to run the server, to run the server from the command line we can use the following command:
```
./srcds_run -game hl2mp -maxplayers 10 +map dm_lockdown
```
After executing this, the server should start runnuing, we can see that the server has started succesfully because of the following output:
```
Connection to Steam servers successful.
   Public IP to Steam is xxx.xxx.xxx.xxx
Assigned anonymous gameserver Steam ID [A:x:xxxxxxxxxx:xxxxx].
VAC secure mode is activated.
```
**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**

## Docker

A docker application is a quick and easy way of deploying an image to do one thing; run a Half-Life 2: Deathmatch Dedicated Server server.

Certain operating systems such as [Unraid](https://unraid.net/) and or [TrueNAS Community Edition](https://www.truenas.com/truenas-community-edition/) allow for application deployement in the shape of docker images from sources such as [Docker Hub](https://hub.docker.com/). Installing such a docker container is relatively simple if you've set up docker containers before, for Unraid and TrueNAS use [this link](https://hub.docker.com/r/ich777/steamcmd/).

A docker container/ application can also be ran on Linux itself and is not difficult to set up, the following steps will explain:

### **Step 1: Installing docker.**
Unlike SteamCMD, Docker cannot be installed from most repositories as far as i know and has to be added, we can do this using the following commands:

First, login as root:

`sudo su`

Run updates:

`sudo apt update`

Fetch the GPG from docker and install it:

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

Fetch the repository list and install it:

`echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

Update once more:

`sudo apt update`

Install docker:

`sudo apt install docker-ce`

Next we start docker and enable it so it starts every time we reboot our machine:

```
sudo systemctl start docker
sudo systemctl enable docker
```

After this, docker should be installed, we can test this by running:

`sudo docker --version`

Which should return:

`Docker versions 28.0.1, build 068a01e`

The version will be different for you most likely.

### **Step 2: Setting up the folder structures.**
We will need to set up 2 folder structures for the Dedicated Server, 1 for the Linux operating system and 1 inside the Docker container, on the Linux system we want the folder structure like this:
```
/home/USERNAME/dockerdata
                         ├─ server
                         │
                         └── stamcmd
```
In this case, on the Linux machine itself we have the `server` and `steamcmd` folder inside `/home/username/dockerdata`. Now we need to set up the folder structure inside the docker container which will look something like this:
```
/dockerdata
           ├─ server
           │
           └── stamcmd
```
These folders don't need to be created as they are created for us when setting up the Docker container.

### **Step 3: Setting up the Docker container.**
First we login as root:

`sudo su`

Next step is to make and run the Docker container, to do that we enter the following string of commands: (**note: it's one long command, the backslash indicates a new line**).
```
docker run --name hl2dm-ds -d \
	-p 27015:27015 -p 27015:27015/udp \
	--env 'GAME_ID=232370' \
	--env 'GAME_NAME=hl2mp' \
	--env 'GAME_PORT=27015' \
	--env 'GAME_PARAMS=+maxplayers 24 +map dm_lockdown' \
	--env 'UID=99' \
	--env 'GID=100' \
	--volume /home/USERNAME/dockerdata/steamcmd:/serverdata/steamcmd \
	--volume /home/USERNAME/dockerdata/server:/serverdata/serverfiles \
	ich777/steamcmd:hl2dm
```
**Note: make sure to replace USERNAME with your username.**

Note: the following lines indicte where the server will be installed to on our Linux machine and their respective path on the Docker container:

`--volume /home/USERNAME/dockerdata/steamcmd:/serverdata/steamcmd \` this indicates that `/home/USERNAME/dockerdata/steamcmd` is linked (`:`) to `/serverdata/steamcmd` in our docker container.

After running the command we should get the following output:
```
Unable to find image 'ich777/steamcmd:hl2dm' locally
latest: Pulling from ich777/steamcmd
69fb10dc82f9: Pull complete 
05ba2b01b3ef: Pull complete 
97289007a3b2: Pull complete 
d8f163d4d5ec: Pull complete 
28f0d6315e16: Pull complete 
1801e57b6599: Pull complete 
Digest: sha256:d5643bce69eadd17ece77cdbebd4ae7716dc8706064be655e70bfa9a95f9917c
Status: Downloaded newer image for ich777/steamcmd:latest
4dde2e97d806e76f9da60fe3cb153c5621b47013f9538572042d3225dde23f6f
```
Here we can see it has pulled all the required data to set up the docker container and we can see our container ID which is the very last line, in this case: `4dde2e97d806e76f9da60fe3cb153c5621b47013f9538572042d3225dde23f6f`, this ID is important as we will be able to identify our Docker container with this. 

### **Step 4: Reading the logs, interacting with docker and other useful commands.**
We can see what the container is doing by reading the log, after the container has been created it will start by itself and start downloading the dedicated server, after this it will start the srver. To list our containers we can use the following command:

`docker ps -a `

This should return something like this:
```
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                                                                              NAMES
4dde2e97d806   ich777/steamcmd:hl2dm   "/opt/scripts/start.…"   8 minutes ago   Up 8 minutes   0.0.0.0:27015->27015/tcp, 0.0.0.0:27015->27015/udp, [::]:27015->27015/tcp, [::]:27015->27015/udp   hl2dm-ds
```
We can see here that our container ID is the same as mentioned above altough shortened, we will need this ID to read the logs which will tell us what it is doing, the command to read the logs is:

`docker logs --since=1h CONTAINER ID`

Which for me would be:

`docker logs --since=1h 4dde2e97d806`

This should return the following output:
```
Connecting anonymously to Steam Public...OK
Waiting for client config...OK
Waiting for user info...OK
---Update Server---
Redirecting stderr to '/serverdata/Steam/logs/stderr.txt'
Logging directory: '/serverdata/Steam/logs'
[  0%] Checking for available updates...
[----] Verifying installation...
UpdateUI: skip show logoSteam Console Client (c) Valve Corporation - version 1738027521
-- type 'quit' to exit --
Loading Steam API...OK
 
Connecting anonymously to Steam Public...OK
Waiting for client config...OK
Waiting for user info...OK
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
 Update state (0x61) downloading, progress: 2.49 (22925547 / 919359275)
 Update state (0x61) downloading, progress: 15.83 (145495935 / 919359275)
 ...
 Update state (0x61) downloading, progress: 94.31 (867026394 / 919359275)
 Update state (0x61) downloading, progress: 98.89 (909133407 / 919359275)
 Update state (0x81) verifying update, progress: 56.00 (514838041 / 919359275)
IPC function call IClientAppManager::GetUpdateInfo took too long: 61 msec
Success! App '232370' fully installed.
---Prepare Server---
mkdir: cannot create directory ‘/serverdata/.steam/sdk32’: No such file or directory
cp: target '/serverdata/.steam/sdk32/' is not a directory
---No 'server.cfg' found, downloading...---
server.cfg          100%[===================>]   1.76K  --.-KB/s    in 0s      
---Successfully downloaded 'server.cfg'---
---Please wait---
---Server ready---
---Start Server---
Auto detecting CPU
Using default binary: ./srcds_linux
Server will auto-restart if there is a crash.
Setting breakpad minidump AppID = 232370
Using breakpad crash handler
dlopen failed trying to load:
/serverdata/.steam/sdk32/steamclient.so
with error:
/serverdata/.steam/sdk32/steamclient.so: cannot open shared object file: No such file or directory
Looking up breakpad interfaces from steamclient
Calling BreakpadMiniDumpSystemInit
03/08 13:33:29 minidumps folder is set to /tmp/dumps
03/08 13:33:29 Could not find steamerrorreporter binary. Any minidumps will be uploaded in-process03/08 13:33:29 Init: Installing breakpad exception handler for appid(232370)/version(9540945)/tid(182)
[S_API] SteamAPI_Init(): Loaded local 'steamclient.so' OK.
Using shader api: shaderapiempty_srv.so
Using Breakpad minidump system. Version: 9540945 AppID: 232370
Loaded 11 VPK file hashes from /serverdata/serverfiles/hl2mp/hl2mp_pak.vpk for pure server operation.
...
Loaded 5 VPK file hashes from /serverdata/serverfiles/platform/platform_misc.vpk for pure server operation.
server_srv.so loaded for "Half-Life 2 Deathmatch"
Parent cvar in server.dll not allowed (hl2mp_bot_debug_ammo_scavenging)
maxplayers set to 24
maxplayers set to 24
ConVarRef dev_loadtime_map_start doesn't point to an existing ConVar
Unknown command "port"
Network: IP 172.17.0.2, mode MP, dedicated Yes, ports 27015 SV / 27005 CL
Initializing Steam libraries for secure Internet server
CAppInfoCacheReadFromDiskThread took 0 milliseconds to initialize
Setting breakpad minidump AppID = 320
SteamInternal_SetMinidumpSteamID:  Caching Steam ID:  76561197960265728 [API loaded yes]
SteamInternal_SetMinidumpSteamID:  Setting Steam ID:  76561197960265728
dlopen failed trying to load:
/serverdata/.steam/sdk32/steamclient.so
with error:
/serverdata/.steam/sdk32/steamclient.so: cannot open shared object file: No such file or directory
Looking up breakpad interfaces from steamclient
Calling BreakpadMiniDumpSystemInit
SteamInternal_SetMinidumpSteamID:  Caching Steam ID:  76561197960265728 [API loaded yes]
SteamInternal_SetMinidumpSteamID:  Setting Steam ID:  76561197960265728
[S_API FAIL] Tried to access Steam interface SteamUtils010 before SteamAPI_Init succeeded.
Setting breakpad minidump AppID = 232370
[S_API FAIL] Tried to access Steam interface SteamNetworkingUtils004 before SteamAPI_Init succeeded.
```
At this point our server is running, we can restart the server using the following command:

`docker restart CONTAINER ID`

And we can stop the container using this command:

`docker stop CONTAINER ID`

**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**
