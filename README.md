# **Half-Life 2: Deathmatch dedicated server guide.**

This guide will explain how to host a dedicated server for Half-Life 2: Deathmatch on the following platforms:
- [Windows](#Windows)
- [Linux](#Linux)
- [Docker Linux](#docker-linux)
- [Docker Desktop Windows](#docker-desktop-windows)

*Docker is, in my opinion, the easiest way to host and manage your server for long term use, docker doesn't use a lot of resources, is easy to interact with and the server is easy to manage.*

**Following this, the guide will explain how to [port forward](#port-forwarding) and [configure](#configuration) your server, configure [FastDL](https://developer.valvesoftware.com/wiki/FastDL) and install addons such as [Metamod](https://www.metamodsource.net/) and [Sourcemod](https://www.sourcemod.net/) with [plugins](#plugins).**

Post installation or if you already have the server installed, use the following links to configure your server further. 
- [Port Forwarding](#port-forwarding)
- [Configuration](#configuration)
- [FastDL](#fastdl)
- [Addons](#addons)
	- [Metamod](https://www.metamodsource.net/)
	- [Sourcemod](https://www.sourcemod.net/)
- [Plugins](#plugins)

# **Windows**

## **Step 1: Setting up your folder structure.**
For the installation of the server we're going to need a folder for [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD) and a folder for the server itself, take the following as an example:
```
Half-Life 2 Deathmatch Dedicated Server/
│
├── steamcmd/
│
└── server/
```
In this example, the Half-Life 2: Deathmatch dedicated server will be installed into the `server` folder and steamCMD, which is used to download the server, will be installed into `steamcmd`. It is up to you where you want to place these folders. 

## **Step 2: Downloading SteamCMD.**
The next step is to download SteamCMD, a download link for Windows can be found [here](https://developer.valvesoftware.com/wiki/SteamCMD#Windows), **make sure you download the one under Windows, the file is called steamcmd.zip .**
Afterwards, unzip this file and put all of its content in the `server` folder, it should now look like this:
```
Half-Life 2 Deathmatch Dedicated Server/
│
├── steamcmd/
│   └── steamcmd.exe
│
└── server/
```

## **Step 3: Running SteamCMD.**
After downloading, we can double click on steamcmd.exe to run it, following this, a terminal window will open which returns the following output:

```
[  0%] Checking for available updates...
[----] Verifying installation...
...
```
After it has downloaded updates, checked for updates and verified the installation we can continue.

## **Step 4: SteamCMD setting installation path.**
Before logging into SteamCMD we must first set the location for the dedicated server to be downloaded and installed to, note that for this we have to use the absolute path.

`force_install_dir \path\to\your\server\`

For example, installing the server onto your D drive would be: 

`force_install_dir D:\Half-Life 2 Deathmatch Dedicated Server\server\`

When setting this on Windows make sure to use the back slash `\` instead of the forward slash `/`.

## **Step 5: Logging in anonymously.**
The next step is to run steamcmd.exe by double clicking on it, this should launch a terminal window running SteamCMD. SteamCMD should update itself if necessary and it should end up with a prompt in which you can type commands. 
We can use the anonymous login feature to download the dedicated server as we don't need to make a purchase to download it, the command to login anonymously is:

`login anonymous`

Following this we should be logged into SteamCMD anonymously, SteamCMD will tell us: 
```
Connecting anonymously to Steam Public...OK
Waiting for client config...OK.
Waiting for user info...OK
```
## **Step 6: Downloading the dedicated server.**
Now that we are logged in and the installation directory is set we can download the dedicated server, to do this we use the following command:

`app_update 232370 validate`

`app_update` will download appid `232370` (Half-Life 2: Deathmatch Dedicated Server) and `validate` the files after downloading.
When the download is complete you may close SteamCMD and your server directory should be populated with files like so:
```
Half-Life 2 Deathmatch Dedicated Server/
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

## **Step 7: Running the dedicated server**
Inside the server directory there should be a file called srcds.exe which will be used to run the server, to do this we make a .bat file to easily run the server and set parameters. The easiest way to do this is to create text file, enter the following command in this file and then save it as .bat instead of a .txt file.  
```
srcds.exe -console -game hl2mp -maxplayers 10 +map dm_lockdown
```
Optionally you can omit `-console` to run the server using a graphical interface. Running the bat file will start the server, we can see that the server is started when the server window tells us:
```
Connection to Steam servers successful.
   Public IP to Steam is xxx.xxx.xxx.xxx
Assigned anonymous gameserver Steam ID [A:x:xxxxxxxxxx:xxxxx].
VAC secure mode is activated.
```
**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**

# Linux

## **Step 1: installing prerequisites.**
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

## **Step 2: Creating a user for SteamCMD.**
Running the server under the `root` user is a *bad idea* and a *scurity risk*, most distributions force you to make a user account which should be used to run the server instead, if you do not have a user account, make one like so:
```
sudo useradd -m newusername
sudo passwd newusername
```

## **Step 3: Setting up your folder structure.**
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

## **Step 4: Running SteamCMD.**
After installing SteamCMD and sertting up the folder structure, we can run SteamCMD with the following command:
`steamcmd`
Following this the prompt should return something like this:
```
[  0%] Checking for available updates...
[----] Verifying installation...
...
```
After it has downloaded updates, checked for updates and verified the installation we can continue.

## **Step 5: Setting your installation directory.**
Before logging into SteamCMD we must first set the location for the dedicated server to be downloaded and installed to, note that for this we have to use the absolute path.

`force_install_dir /path/to/your/server`

For example, installing the server into your home folder (recommended, permission will be set accordingly) 

`force_install_dir /home/username/hl2dm-ds/server`

When setting this on Linux make sure to use the forward slash `/` instead of the back slash `\`.

## **Step 6: Logging in anonymously.**
The next step is to login to SteamCMD using an anonymous account as we do not need to purchase HL2:DM or anything to be able to download the server, to login, use the following command:

`login anonymous`

Following this we should be logged into SteamCMD anonymously, SteamCMD will tell us: 
```
Connecting anonymously to Steam Public...OK
Waiting for client config...OK.
Waiting for user info...OK
```

## **Step 7: Downloading the dedicated server.**
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

## **Step 8: Running the dedicated server**
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

# Docker (Linux)

A docker application is a quick and easy way of deploying an image to do one thing; run a Half-Life 2: Deathmatch Dedicated Server server. A docker container is simple to manage and configuration is easy. 

Certain operating systems such as [Unraid](https://unraid.net/) and or [TrueNAS Community Edition](https://www.truenas.com/truenas-community-edition/) allow for application deployement in the shape of docker images from sources such as [Docker Hub](https://hub.docker.com/). Installing such a docker container is relatively simple if you've set up docker containers before, for Unraid and TrueNAS use [this link](https://hub.docker.com/r/ich777/steamcmd/).

A docker container/ application can also be ran on Linux itself and is not difficult to set up, the following steps will explain:

## **Step 1: Installing docker.**
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

## **Step 2: Setting up the folder structures.**
We will need to set up 2 folder structures for the Dedicated Server, 1 for the Linux operating system and 1 inside the Docker container, on the Linux system we want the folder structure like this:
```
/home/USERNAME/dockerdata
                         ├─ server
                         │
                         └─ stamcmd
```
In this case, on the Linux machine itself we have the `server` and `steamcmd` folder inside `/home/username/dockerdata`. Now we need to set up the folder structure inside the docker container which will look something like this:
```
/dockerdata
           ├─ server
           │
           └── stamcmd
```
These folders don't need to be created as they are created for us when setting up the Docker container.

## **Step 3: Setting up the Docker container.**
First we login as root:

`sudo su`

Then, go to the docker directory on your Linux machine, in the example shown above that would be `/home/USERNAME/dockerdata`, to do this, use:

`cd /home/USERNAME/dockerdata` (make sure to change USERNAME with your username)

Then, make `docker-compose.yaml` using the following commands:

`touch docker-compose.yaml`

Open `docker-compose.yaml` using the text editor with:

`nano docker-compose.yaml` (or your text editor)

Then, add the following lines to your `docker-compose.yaml` file:

```
services:
  hl2dm:
    container_name: hl2dm-ds
    image: ich777/steamcmd:hl2dm
    restart: unless-stopped
    ports:
      - "27015:27015"
      - "27015:27015/udp"
    environment:
      GAME_ID: 232370
      GAME_NAME: hl2mp
      GAME_PORT: 27015
      GAME_PARAMS: "+maxplayers 24 +map dm_lockdown"
      UID: 99
      GID: 100
    volumes:
      - /home/USERNAME/dockerdata/steamcmd:/serverdata/steamcmd
      - /home/USERNAME/dockerdata/server:/serverdata/serverfiles
```
**Note: make sure to change USERNAME with your username!**

Note: the following lines indicte where the server will be installed to on our Linux machine and their respective path on the Docker container:

`- /home/USERNAME/dockerdata/steamcmd:/serverdata/steamcmd` this indicates that `/home/USERNAME/dockerdata/steamcmd` is linked (`:`) to `/serverdata/steamcmd` in our docker container.

## **Step 4: Running the container**
Some parts of our server configuration are written inside the `docker-compose.yaml` file such as the port the server will use and the server parameters including the starting map, this file can be updated when the server is turned of in case you ever need to change these. Next we can start our container using the following command:

`docker compose up -d`

After running the command and letting the images download, we should get the following output:
```
[+] Running 7/7
 ✔ hl2dm Pulled                                                                                                                    5.6s
   ✔ 69fb10dc82f9 Pull complete                                                                                                    3.8s
   ✔ 05ba2b01b3ef Pull complete                                                                                                    4.0s
   ✔ dc2b3bbca85d Pull complete                                                                                                    4.0s
   ✔ 8a56348ea875 Pull complete                                                                                                    4.0s
   ✔ 1df2777fc818 Pull complete                                                                                                    4.0s
   ✔ 6e694c0deac5 Pull complete                                                                                                    4.0s
[+] Running 2/2
 ✔ Network dockerdata_default  Created                                                                                             0.0s
 ✔ Container hl2dm-ds          Started                                                                                             0.2s
```
Here we can see it has pulled all the required data to set up the docker container and we can see our container name is `hl2dm-ds`, this name will be important to identify our container.

## **Step 5: Reading the logs, interacting with docker and other useful commands.**
We can see what the container is doing by reading the log, after the container has been created it will start by itself and start downloading the dedicated server, after this it will start the server. To list our containers we can use the following command:

`docker ps -a `

This should return something like this:
```
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                                                                              NAMES
4dde2e97d806   ich777/steamcmd:hl2dm   "/opt/scripts/start.…"   8 minutes ago   Up 8 minutes   0.0.0.0:27015->27015/tcp, 0.0.0.0:27015->27015/udp, [::]:27015->27015/tcp, [::]:27015->27015/udp   hl2dm-ds
```
We can open the logs from our container with the following command:

`docker compose logs -f hl2dm-ds`

This should return the following output:
```
hl2dm-ds  | ---Ensuring UID: 99 matches user---
hl2dm-ds  | ---Ensuring GID: 100 matches user---
hl2dm-ds  | ---Setting umask to 000---
hl2dm-ds  | ---Checking for optional scripts---
hl2dm-ds  | ---No optional script found, continuing---
hl2dm-ds  | ---Taking ownership of data...---
hl2dm-ds  | ---Starting...---
hl2dm-ds  | SteamCMD not found!
hl2dm-ds  | steamcmd.sh
hl2dm-ds  | linux32/steamcmd
hl2dm-ds  | linux32/steamerrorreporter
hl2dm-ds  | linux32/libstdc++.so.6
hl2dm-ds  | linux32/crashhandler.so
hl2dm-ds  | ---Update SteamCMD---
hl2dm-ds  | Redirecting stderr to '/serverdata/Steam/logs/stderr.txt'
hl2dm-ds  | ILocalize::AddFile() failed to load file "public/steambootstrapper_english.txt".
hl2dm-ds  | [  0%] Checking for available update...
hl2dm-ds  | [----] Downloading update (0 of 49,478 KB)...
hl2dm-ds  | [  0%] Downloading update (0 of 49,478 KB)...
hl2dm-ds  | [  0%] Downloading update (0 of 49,478 KB)...
hl2dm-ds  | [  0%] Downloading update (0 of 49,478 KB)...
hl2dm-ds  | [  0%] Downloading update (0 of 49,478 KB)...
hl2dm-ds  | [  0%] Downloading update (1,018 of 49,478 KB)...
hl2dm-ds  | [  2%] Downloading update (2,225 of 49,478 KB)...
...
hl2dm-ds  | [----] Extracting package...
...
hl2dm-ds  | [----] Installing update...
hl2dm-ds  | [----] Cleaning up...
hl2dm-ds  | [----] Update complete, launching...
hl2dm-ds  | Redirecting stderr to '/serverdata/Steam/logs/stderr.txt'
hl2dm-ds  | Logging directory: '/serverdata/Steam/logs'
hl2dm-ds  | [  0%] Checking for available updates...
hl2dm-ds  | [----] Verifying installation...
hl2dm-ds  | [  0%] Downloading update...
hl2dm-ds  | [  0%] Checking for available updates...
hl2dm-ds  | [----] Download complete.
hl2dm-ds  | [----] Extracting package...
...
hl2dm-ds  | [----] Installing update...
hl2dm-ds  | [----] Cleaning up...
hl2dm-ds  | [----] Update complete, launching Steamcmd...
hl2dm-ds  | UpdateUI: skip show logo
hl2dm-ds  | steamcmd.sh[33]: Restarting steamcmd by request...
hl2dm-ds  | Redirecting stderr to '/serverdata/Steam/logs/stderr.txt'
hl2dm-ds  | Logging directory: '/serverdata/Steam/logs'
hl2dm-ds  | [  0%] Checking for available updates...
hl2dm-ds  | [----] Verifying installation...
hl2dm-ds  | UpdateUI: skip show logo
hl2dm-ds  | Steam Console Client (c) Valve Corporation - version 1741737873
hl2dm-ds  | -- type 'quit' to exit --
hl2dm-ds  | Loading Steam API...OK
hl2dm-ds  |
hl2dm-ds  | Connecting anonymously to Steam Public...OK
hl2dm-ds  | Waiting for client config...OK
hl2dm-ds  | Waiting for user info...OK
hl2dm-ds  | ---Update Server---
hl2dm-ds  | Redirecting stderr to '/serverdata/Steam/logs/stderr.txt'
hl2dm-ds  | Logging directory: '/serverdata/Steam/logs'
hl2dm-ds  | [  0%] Checking for available updates...
hl2dm-ds  | [----] Verifying installation...
hl2dm-ds  | UpdateUI: skip show logo
hl2dm-ds  | Steam Console Client (c) Valve Corporation - version 1741737873
hl2dm-ds  | -- type 'quit' to exit --
hl2dm-ds  | Loading Steam API...OK
hl2dm-ds  |
hl2dm-ds  | Connecting anonymously to Steam Public...OK
hl2dm-ds  | Waiting for client config...OK
hl2dm-ds  | Waiting for user info...OK
hl2dm-ds  |  Update state (0x3) reconfiguring, progress: 0.00 (0 / 0)
...
hl2dm-ds  |  Update state (0x61) downloading, progress: 0.19 (1717150 / 919359275)
hl2dm-ds  |  Update state (0x61) downloading, progress: 2.39 (22009796 / 919359275)
hl2dm-ds  |  Update state (0x61) downloading, progress: 3.83 (35255756 / 919359275)
....
hl2dm-ds  |  Update state (0x61) downloading, progress: 99.77 (917262123 / 919359275)
hl2dm-ds  |  Update state (0x61) downloading, progress: 99.77 (917262123 / 919359275)
hl2dm-ds  |  Update state (0x81) verifying update, progress: 4.33 (39775643 / 919359275)
hl2dm-ds  |  Update state (0x81) verifying update, progress: 18.78 (172636393 / 919359275)
...
hl2dm-ds  |  Update state (0x81) verifying update, progress: 87.07 (800489212 / 919359275)
hl2dm-ds  | IPC function call IClientAppManager::GetUpdateInfo took too long: 42 msec
hl2dm-ds  | Success! App '232370' fully installed.
hl2dm-ds  | ---Prepare Server---
hl2dm-ds  | mkdir: cannot create directory ‘/serverdata/.steam/sdk32’: No such file or directory
hl2dm-ds  | cp: target '/serverdata/.steam/sdk32/' is not a directory
hl2dm-ds  | ---No 'server.cfg' found, downloading...---
server.cfg          100%[===================>]   1.76K  --.-KB/s    in 0s
hl2dm-ds  | ---Successfully downloaded 'server.cfg'---
hl2dm-ds  | ---Please wait---
hl2dm-ds  | ---Server ready---
hl2dm-ds  | ---Start Server---
hl2dm-ds  | Auto detecting CPU
hl2dm-ds  | Using default binary: ./srcds_linux
hl2dm-ds  | Server will auto-restart if there is a crash.
hl2dm-ds  | Setting breakpad minidump AppID = 232370
hl2dm-ds  | Using breakpad crash handler
hl2dm-ds  | dlopen failed trying to load:
hl2dm-ds  | /serverdata/.steam/sdk32/steamclient.so
hl2dm-ds  | with error:
hl2dm-ds  | /serverdata/.steam/sdk32/steamclient.so: cannot open shared object file: No such file or directory
hl2dm-ds  | Looking up breakpad interfaces from steamclient
hl2dm-ds  | Calling BreakpadMiniDumpSystemInit
hl2dm-ds  | 03/15 18:53:20 minidumps folder is set to /tmp/dumps
hl2dm-ds  | 03/15 18:53:20 Could not find steamerrorreporter binary. Any minidumps will be uploaded in-process03/15 18:53:20 Init: Installing breakpad exception handler for appid(232370)/version(9540945)/tid(182)
hl2dm-ds  | [S_API] SteamAPI_Init(): Loaded local 'steamclient.so' OK.
hl2dm-ds  | Using shader api: shaderapiempty_srv.so
hl2dm-ds  | Using Breakpad minidump system. Version: 9540945 AppID: 232370
hl2dm-ds  | Loaded 11 VPK file hashes from /serverdata/serverfiles/hl2mp/hl2mp_pak.vpk for pure server operation.
hl2dm-ds  | Loaded 11 VPK file hashes from /serverdata/serverfiles/hl2mp/hl2mp_pak.vpk for pure server operation.
hl2dm-ds  | Loaded 1 VPK file hashes from /serverdata/serverfiles/hl2_complete/hl2_complete_misc.vpk for pure server operation.
hl2dm-ds  | Loaded 1231 VPK file hashes from /serverdata/serverfiles/hl2/hl2_textures.vpk for pure server operation.
hl2dm-ds  | Loaded 574 VPK file hashes from /serverdata/serverfiles/hl2/hl2_sound_vo_english.vpk for pure server operation.
hl2dm-ds  | Loaded 383 VPK file hashes from /serverdata/serverfiles/hl2/hl2_sound_misc.vpk for pure server operation.
hl2dm-ds  | Loaded 462 VPK file hashes from /serverdata/serverfiles/hl2/hl2_misc.vpk for pure server operation.
hl2dm-ds  | Loaded 5 VPK file hashes from /serverdata/serverfiles/platform/platform_misc.vpk for pure server operation.
hl2dm-ds  | server_srv.so loaded for "Half-Life 2 Deathmatch"
hl2dm-ds  | Parent cvar in server.dll not allowed (hl2mp_bot_debug_ammo_scavenging)
hl2dm-ds  | maxplayers set to 24
hl2dm-ds  | maxplayers set to 24
hl2dm-ds  | ConVarRef dev_loadtime_map_start doesn't point to an existing ConVar
hl2dm-ds  | Unknown command "port"
hl2dm-ds  | Network: IP 172.19.0.2, mode MP, dedicated Yes, ports 27015 SV / 27005 CL
hl2dm-ds  | Initializing Steam libraries for secure Internet server
hl2dm-ds  | CAppInfoCacheReadFromDiskThread took 0 milliseconds to initialize
hl2dm-ds  | Setting breakpad minidump AppID = 320
hl2dm-ds  | SteamInternal_SetMinidumpSteamID:  Caching Steam ID:  76561197960265728 [API loaded yes]
hl2dm-ds  | SteamInternal_SetMinidumpSteamID:  Setting Steam ID:  76561197960265728
hl2dm-ds  | dlopen failed trying to load:
hl2dm-ds  | /serverdata/.steam/sdk32/steamclient.so
hl2dm-ds  | with error:
hl2dm-ds  | /serverdata/.steam/sdk32/steamclient.so: cannot open shared object file: No such file or directory
hl2dm-ds  | Looking up breakpad interfaces from steamclient
hl2dm-ds  | Calling BreakpadMiniDumpSystemInit
hl2dm-ds  | SteamInternal_SetMinidumpSteamID:  Caching Steam ID:  76561197960265728 [API loaded yes]
hl2dm-ds  | SteamInternal_SetMinidumpSteamID:  Setting Steam ID:  76561197960265728
hl2dm-ds  | [S_API FAIL] Tried to access Steam interface SteamUtils010 before SteamAPI_Init succeeded.
hl2dm-ds  | Setting breakpad minidump AppID = 232370

```
We can see above that the container has downloaded and updated SteamCMD, downloaded and installed the dedicated server and started the server. At this point our server is running, we can restart the container using the following command:

`docker restart hl2dm-ds`

And we can stop the container using this command:

`docker stop hl2dm-ds`

**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**

# Docker Desktop (Windows)

## **Step 1: Installing and running:**
[Docker Desktop](https://www.docker.com/products/docker-desktop/) is an easy way of running Docker containers on Windows, to do this, simply go on their website and download Docker desktop (it's free). After downloading and installing Docker desktop let it install and or update certain [WSL](https://learn.microsoft.com/en-us/windows/wsl/about) components. After all of this is done, open Docker desktop and click on the `>_ Terminal` button in the bottom right and enable it, then in the terminal enter the following command:
 ```
docker run --name hl2dm-ds -d -p 27015:27015 -p 27015:27015/udp --env 'GAME_ID=232370' --env 'GAME_NAME=hl2mp' --env 'GAME_PORT=27015' --env 'GAME_PARAMS=+maxplayers 24 +map dm_lockdown' --env 'UID=99' --env 'GID=100' --volume C:\Half-Life 2 Deathmatch Dedicated Server\steamcmd:/serverdata/steamcmd --volume C:\Half-Life 2 Deathmatch Dedicated Server\server:/serverdata/serverfiles ich777/steamcmd:hl2dm
``` 
After running this, Docker desktop should start to pull the image as it is not locally stored and start the container. On the top left side there should be a "Containers" section, clicking on this we should see our container named "hl2dm-ds", double clicking on this opens the log and we can see what the container is doing. We can see in the command that we have mounted the Windows path `C:\Half-Life 2 Deathmatch Dedicated Server` to be used for our server files which will contain the `steamcmd` and `serverfiles` contents.

**AT THIS POINT YOUR SERVER IS RUNNING, BUT FOR OTHERS TO BE ABLE TO JOIN IT YOU WILL NEED TO [PORT FORWARD](portforward) AND DO BASIC [CONFIGURATION](configuration)**

# Port forwarding
Port forwarding is required for to be done for others to be able to join your server from the internet, without port forwarding your server will **not show up in the server list** and people who are not on the same network as you are will **not be able to join**. Port forwarding is different for each and every set up, this guide will assume you're hosting a dedicated server from a home network and that you are using a home router and or firewall, if you're paying for a VSP or another server space you will have to figure out with them how to open ports to allow for game traffic from the internet. 

## **Step 1: Finding your router IP adres.**
To find our router's IP adres on windows we can open the command prompt and use the following command:

`ipconfig`

Which will return:

```
Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . : home
   IPv4 Address. . . . . . . . . . . : xxx.xxx.xxx.xxx
   Subnet Mask . . . . . . . . . . . : xxx.xxx.xxx.xxx
   Default Gateway . . . . . . . . . : 192.168.1.1
```
In this example, my default gateway which is my router, has the IP adres `192.168.1.1`. To do this on Linux we can use the following command:

`netstat -rn`

Which will return:

```
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG        0 0          0 ens2
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 ens2
```

In this case we can see that our router's IP adres (gateway) is `192.168.1.1`

## **Step 2: Logging into your router's web interface and opening your port.**
Open a web browser and enter the your gateway's IP adres, this should give you a login page to enter your router's web login. The password for this web login is usually found on the bottom of your router and is not the same as the Wi-Fi password.

After logging in we must find the port forwarding options, for me they can be found under `Network > NAT > Port mapping`. Next step is to add a port mapping/ port forwarding rule, the IP adres to use for this rule is th IP adres of the computer that your server is hosted on, for example this could be `192.168.1.10`, next we must set the port to 27015 (or whatever port you're using) and the protocol to UDP/TCP (both). An example port forwarding rule may look something like this:

```
Enable this rule: ☒
IP: 192.168.1.10
Protocol: TCP/UDP
Private Port: 27015 - 27015
Public Port: 27015 - 27015
```
After doing this, start your server and it should now be joinable from the server list but, we haven't changed configuration yet which is required for changing the name of the server and optionally setting a password, without changing the name it will be very difficult to find your server in the list. 

## Configuration. 
Server configuration is mostly stored in the `server.cfg` file, this configuration file contains a list of commands that the server executes when it starts up and is ran every time the server starts.

## **Step 1: Server configuration.**
The `server.cfg` file exists in `hl2mp/cfg/server.cfg`, if it's not there, you can create it yourself and add the following text:
```
// Basic server settings
// Name of the server
hostname "My Half-Life 2 DM Server"

// Set a password for the server if required (leave blank for no password)
sv_password ""

// Remote console password (change it to something secure)
rcon_password "your_rcon_password"

// Set to 1 to allow cheats (set to 0 to disable)
sv_cheats 0

// Set to 1 for LAN-only play, set to 0 for internet play
sv_lan 0

// Server contact email (optional)
sv_contact "your_email@example.com"

// Region 0 for worldwide, change it depending on your location (use Steam region numbers)
sv_region 0

// Disable auto-kick (0 = enabled, 1 = disabled)
mp_disable_autokick 0

// Gameplay settings
// Weapon respawn time in seconds
sv_hl2mp_weapon_respawn_time 20

// Item respawn time in seconds
sv_hl2mp_item_respawn_time 30

// set to 1 if weapons stay (immediate pickup by players without weapons)
// requires that there be additional ammo (can't pick up a weapon to get more ammo)
mp_weaponstay 0

// enable autocrosshair (default is 1)
mp_autocrosshair 1

// teamplay talk all (1) or team only (0)
sv_alltalk 1

//toggles whether the server allows spectator mode or not
mp_allowspectators 1

// enable voice on server
sv_voiceenable 1

// set to force players to respawn after death
mp_forcerespawn 1

// Time limit per map (in minutes)
mp_timelimit 20

// Round time limit (in minutes)
mp_roundtime 3

// Set the win limit (0 = unlimited)
mp_winlimit 0

// Max rounds per map (0 = unlimited)
mp_maxrounds 0

// Allows a team to be unbalanced by this number of players
mp_teams_unbalance_limit 0

// Set to 1 to allow friendly fire, 0 to disable
mp_friendlyfire 0

// Enable or disable flashlight (1 = enabled, 0 = disabled)
mp_flashlight 1

// Enable or disable automatic team balancing (1 = enabled)
mp_autoteambalance 0

// Automatically kicks idle players
mp_autokick 0

// Restarts the game when round ends
mp_restartgame 1

// Automatically restarts the game at the end of each match
mp_match_end_restart 1

// Allow monsters (set to 0 for deathmatch)
mp_allowmonsters 0

// Forces auto-kicking when players are idle
mp_forceautokick 0

// Player settings
// Time before a player is kicked for not responding (in seconds)
sv_timeout 60

// Set the acceleration while noclipping
sv_noclipaccelerate 5

// Set the speed while noclipping
sv_noclipspeed 5

// Miscellaneous settings
// URL for fast download (leave empty or specify URL)
sv_downloadurl ""

// Forces all players to have a 3rd-person view (1 = enabled)
mp_forcecamera 1

// Map rotation
// Map rotation file (make sure to configure it in a separate file)
mapcyclefile "mapcycle.txt"

// Server security settings
// Allow clients to upload custom files (set to 0 to disable)
sv_allowupload 1

// Allow clients to download custom files (set to 0 to disable)
sv_allowdownload 1

// Time (in minutes) a player is banned after a failed rcon attempt
sv_rcon_banpenalty 10

// Number of failed rcon attempts before banning
sv_rcon_maxfailures 5

// Map-related settings
// Time for players to chat after the game ends (in seconds)
mp_chattime 5
``` 
Feel free to change any of these settings as you like, most importantly, probably, would be `hostname`, set this to something that represents your server, make it unique so you and any other player can easily find your server. Aside from this there are a lot more commands that can be used to alter server configuration, a list of convars can be found [here](https://developer.valvesoftware.com/wiki/List_of_Half-Life_2_console_commands_and_variables).

# FastDL
[FastDL](https://developer.valvesoftware.com/wiki/FastDL) is used by many source games to download maps, models and other game assets that are used on a server. If you're hosting a server with custom maps or models, the people who join your server will need to download these as they are not automatically downloaded, to do this we must set up a `sv_downloadurl`. A `sv_downloadurl` is a link to a website that hosts all the custom assets that are used on your server. You can host a downloadurl yourself by purchasing a domain and hosting a website using this domain or hosting one locally and access it via your public IP adres, if you're paying for server hosting it is likely that your server host has web space available to be used as a downloadurl. The following steps will explain how to set one up correctly assuming you're hosting it yourself or on a domain.

## **Step 1: Setting up your downloadurl.**
First of all we must make a browsable directory on your webserver, this example will show how this is done in Linux as i don't know how Window's ISS works. Let's assume your website is hosted in the directory `/var/www/html/public`, we can make another folder next to it in which our game files are hosted like so:
```
/var/www/html/
             ├─ public
             │
             └── hl2mp
```
Then we must specify that this folder should be browsable, that way your browser or game will not try to read it as a website but as a directory listing. The following example shows how this is done using Apache2, append this in your directory block:
```
<Directory /var/www/html/hl2mp>
    Options +Indexes
</Directory>
```
If you're using Nginx, add this to your website's configuration:
```
location /var/www/html/hl2mp/ {
    autoindex on;
    autoindex_exact_size off;  # Optional: Display file sizes in a human-readable format
    autoindex_localtime on;    # Optional: Display file times in the server's local time
}
```
Now you should be able to browser your /hl2mp/ directory in your browser by going to yourwebsite.com/hl2mp.
 
## **Step 2: Populating and using your downloadurl.**
When adding maps, models, materials or sounds or any other custom models to your download url it is important that this folder structure is the same on your dedicated server, for example, if your game directory looks like this:
```
/home/USERNAME/dockerdata/server/hl2mp/maps/
					   ├─ custommap1.bsp
					   └── custommap2.bsp
```
Then your download url directory on your web server should look like this:
```
/var/www/html/hl2mp/maps/
                        ├─ custommap1.bsp
                        └── custommap2.bsp
```
**Note that, everything within the hl2mp server should look the same and should be structured the same on both the web server and the dedicated server.**

Finally, we must set our `sv_downloadurl` in our server configuration, find your server.cfg file in `hl2mp/cfg` and edit `sv_downloadurl` to `sv_downloadurl "yourwebsite.com/hl2mp"` and then restart your server for it to take effect. Now whenever you load a non-standard map on your server, anyone joining should download this map automatically as long as the maps are both on your web server and on your dedicated server.  

# Addons
Addons can be installed on your server to allow for plugin loading, adding admins and further server customization, it is highly recommended to install addons on your dedicated server. This guide will explain how to install [Metamod](https://www.sourcemm.net/) and [Sourcemod](https://www.sourcemod.net/downloads.php), Sourcemod allows for plugin loading and Metamod is needed for Sourcemod to load. 

**Note: as of writing this guide, the most recent compatible versions of Metamod and Sourcemod for Half-Life 2: Deathmatch are:**

 **- Metamod:Source version 2.0.0-dev+1333**

 **- SourceMod Version: 1.12.0.7190**

## **Step 1. Installing Metamod**
Go to the [Metamod download](https://www.sourcemm.net/downloads.php?branch=stable) page and download a version compatibale with your server, you may use the server version above as an example but these may be outdated. 

**Note: make sure to download the Windows version if your using the Windows guide, if you're using the Linux or Docker guides make sure to download the Linux version!**

Next, unzip your file and move the addons folder into your `hl2mp` directory, on Windows your server will look something like this:
```
C:\path\to\your\server\hl2mp\
			     └── addons\
```
If youre using Docker on Windows you will have to use the path you chose when creating the docker container:
```
C:\path\to\your\docker\server\hl2mp\
				    └── addons\
```
On Linux it will look something like this:
```
/path/to/your/server/hl2mp/
			   └── addons/
```
If you're using Docker on Linux you will have to use the path you chose when creating the docker container:
```
/path/to/your/docker/server/hl2mp/
			          └── addons/
```
## **Step 2: Using Metamod**
Metamod is installed after moving the addons folder into your hl2mp directory, following this we can restart the server and Metamod should be loaded after the restart. We can check if Metamod is loaded by joining the server and using `meta version` in the console which should return something like:

```
    Metamod:Source version 2.0.0-dev+1333
 Metamod:Source Version Information
    Plugin interface version: 17:14
    SourceHook version: 5:5
    Loaded As: Valve Server Plugin
    Built from: https://github.com/alliedmodders/metamod-source/commit/d6ed444
    Compiled on: Feb 20 2025 14:36:31
    Build ID: 1333:d6ed444
    http://www.metamodsource.net/
```

**Note: if it says the command is not found this means Metamod is not installed, this is required for Sourcemod to work and for plugins to function. You may try different versions and or branches of Metamod until you find one that works on your server.**
