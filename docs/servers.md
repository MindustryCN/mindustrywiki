# 服务器

服务器是Mindustry的重要组成部分，因为它们提供了与其他人一起玩游戏的能力。服务器主要有两种类型：专用服务器和本地局域网服务器。

## 专用服务器
专用服务器是独立的，无界面的软件，只专注于提供一种手段，让人们玩多人游戏。它们通常作为一个独立的程序在计算机上运行，而不是在游戏中运行，并从终端进行操作。它们通常比本地局域网服务器更强大，因为它们有更多的可用资源来支持两个或以上的玩家，并且可以全天候运行。它们也更通用和强大，因为它们有许多命令来为管理员提供更多的控制，并且它们可以很容易地进行修改以满足管理员的需要。

您可以使用“播放”菜单下的“加入游戏”按钮连接到服务器。与本地局域网服务器不同，您必须输入主机的IP地址和端口。与本地局域网服务器不同的是，一旦你添加了一个服务器，当你打开它时，它会自动出现在你的服务器列表上，游戏会自动检查服务器的状态。

To establish a dedicated server, a dedicated Linux or Windows machine is **highly** recommended.

1. If you haven't already, install at least JRE and JDK 8.
2. Download the desired server release from [itch.io](https://anuke.itch.io/mindustry), or compile one yourself. 
3. Open a terminal or TTY session then change `cd` to the directory the JAR is placed in.
4. Run `java -jar server.jar` using Command Prompt (on Windows) or your favorite terminal (on Linux and Mac). The commands are explained in the `help` command.
5. Start hosting a map with `host <mapname> [mode]` after you configured your server.
6. If you are using Windows to run your server, use your favorite search engine to look up how to add rules to your Windows Firewall, as it blocks that port most of the time. Make sure to allow **port 6567 TCP and UDP**.

Unless you have already enabled port forwarding, your dedicated server can only be connected to by clients within your local network. If you want to make your server globally available, read below.

### What is an IP and how do I find out what mine is?

In simplified terms, an IP address is a number that identifies your computer on the internet. You can connect to someone's Mindustry server if you know their IP address. There are two types; a **public** and a **local** address.

- For a **local IP**, when for example you would like to play with a friend on the same network as yours, each device has its own way of showing it. You can Google how to do it for your device, e.g. "find local ip on Mac".
- For a **public IP**, you can simply Google "what is my ip".

### Running A Dedicated Server At Home

Most of the time, this is what you should remember; **never share your public IP with the public if you're hosting from your home, unless you acknowledge the implications of doing so!** Your public IP is tied to your household, and if it falls into the wrong hands, and when put into the wrong hands, can open up your network to vulnerabilities and dangers. **Exercise caution, do your research, and use a VPN or webhost if possible.**

It is also recommended and that you use a domain name or DNS service to mask your IP for public servers for ease of use, or even better, use a cloud service e.g. Amazon AWS or a dedicated server/VM from a hosting provider such as Linode or DigitalOcean, which is much safer. **Do your research**, and determine which option best fits your needs.

1. Find the make/model of your router. This is usually on a sticker on the bottom or back of the router.
2. Use your favorite search engine to search "port forward ASUS RT-ACRH17" and use the guide to foward **port 6567 TCP and UDP**. These instructions are different for every router, so be sure to read your guide thoroughly!
3. You can use a service such as [You Get Signal](https://www.yougetsignal.com/tools/open-ports/) to check if you have done your portforwarding correctly. 

## Local LAN & Steam Servers

A local LAN or Steam server is a server that is built into the game, and can be started using the "Host Multiplayer Game" button in the in-game menu. It is meant to be simple and straightforward, for sessions between a few players under a LAN network (aka in your household's WiFi network). It is not really meant for several players, as it takes more and more resources from your device to be able to use it that way; for that you will need a dedicated server mentioned above. It can only run when the game is open, and is immediately terminated when it is closed.

You can connect to one using the "Join Game" button under the "Play" menu. Unlike dedicated servers, your device will automatically find the host device and it will ususally appear in the server list without you having to enter the host's IP address in.

## Dedicated Server Commands

$serverCommands

## Dedicated Server Configuration Options

$serverConfigs
