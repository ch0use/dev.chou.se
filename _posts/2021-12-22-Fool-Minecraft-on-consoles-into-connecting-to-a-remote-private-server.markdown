---
layout: post
title:  "Fool Minecraft on consoles into connecting to a remote private server!"
date:   2021-12-22 00:00:00 -0400
categories: minecraft
---
Minecraft on Xbox and other Consoles only support connecting to local LAN private servers or official ones over the Internet. If you want to connect Minecraft on a console to a remote private Minecraft server, you need a method to fool it into thinking the remote server is actually local.

If you host a [Minecraft Bedrock](https://minecraft.fandom.com/wiki/Bedrock_Edition) server through a hosting service, no one with an Xbox or other console (including you) can join it from the console and there is [no ability to type in its IP address and connect manually](https://minecraft.fandom.com/wiki/Bedrock_Edition#:~:text=Joining%20servers%20through%20IP%20isn%27t%20supported%20by%20Xbox%20One%2C%20Nintendo%20Switch%2C%20or%20PlayStation%204.).

If you host your own Minecraft Bedrock server in your house and you have an Xbox or other console, it will show up for you in the Friends tab in Minecraft on the console, but friends using a console in their house will not be able to see it in their Friends tab, even if you are in the game and try to invite them.

*If interested, to learn how to set up your own Bedrock server on Ubuntu linux, check out [Minecraft Bedrock Edition -- Ubuntu Dedicated Server Guide](https://jamesachambers.com/minecraft-bedrock-edition-ubuntu-dedicated-server-guide/). If you run this on a server at your house, be sure to review the [Port Forwarding](https://jamesachambers.com/minecraft-bedrock-edition-ubuntu-dedicated-server-guide/#:~:text=safe%20to%20run!-,Port%20Forwarding,-If%20everyone%20on) section to port-forward UDP:19132 to your internal Minecraft server.*

In order to get Minecraft on Xbox or other consoles on the local network to connect to a private Minecraft Bedrock server over the internet, we first need to understand how Minecraft discovers local servers. I have not done packet capture analysis to validate this theory, but it would appear that Minecraft scans the local subnet, making connection attempts against all local hosts on UDP 19132, looking for valid responses.

If we run a service on a host on the local network that listens on UDP 19132 and forwards any internal packets sent to it out over the internet to the real Minecraft Bedrock server, the console should believe it has found a local Minecraft server and list it in the Friends tab.

The following method uses a Windows laptop connected to the same local network as the console running Minecraft. The laptop runs a service which listens on UDP 19132 and forwards connection requests to a remote Minecraft Bedrock server over the internet.

To Minecraft on the console, it will show a LAN server in the Friends tab which is actually the laptop, forwarding to the remote Minecraft Bedrock server.

Follow these steps to get your console running Minecraft to connect to a remotely-hosted Bedrock server, whether it's at a friend's house, or a hosting service.

If you have the Bedrock server running locally, your remote friend will need to perform these steps, not you.

[Download the sudppipe utility](http://aluigi.altervista.org/mytoolz/sudppipe.zip) which will listen on UDP 19132 and forward requests to the Bedrock server. Many thanks to the developer [Luigi Auriemma](http://aluigi.altervista.org/) who has made this method possible.

-   The utility is [mirrored locally here](https://chou.se/wp-content/uploads/2021/12/sudppipe.zip) in case the original location is no longer accessible.

1.  Open your Downloads folder
2.  Right click on sudppipe.zip and choose Extract All
3.  Change the path to be C:\sudppipe
4.  Open the Start menu and start typing "command" and then open the Command Prompt app when it shows up in the search
5.  Type the following commands in the Command Prompt
    1.  `cd \sudppipe`
    2.  `sudppipe.exe server.hostname.com 19132 19132`
        -   Replace _server.hostname.com_ with the DNS hostname or IP address of the Minecraft Bedrock server. Leave the default UDP ports of 19132.
    3.  A Windows Security Alert may pop up asking if it's OK for this application to communicate on networks.
        1.  Check the boxes for Domain, Private, and Public networks, then click Allow Access
            -   This is safe to do because it only applies to your home network and only when the sudppipe program is running.
6.  Leave the command prompt window open with `sudppipe` running
7.  In Minecraft on the console, go to Play and then select the Friends tab
8.  If everything is working properly, a new LAN game should now be listed -- this is the Minecraft Bedrock server.
9.  You should be able to connect and join the game.
10. When done, on the laptop in the Command Prompt box, press CTRL+C to quit `sudppipe`. The console won't be able to see or communicate with the Bedrock server anymore.

![](https://i0.wp.com/chou.se/wp-content/uploads/2021/12/sudppipe.png?resize=422%2C185&ssl=1)

Next time, just open a Command Prompt again and follow the steps above starting at Step 4 and run the commands again. If savvy enough, you can make a batch file script which performs the steps for you, and then just run the batch file.

This was validated with a Windows 10 laptop and Xbox One console but has not been tested with Nintendo Switch or PlayStation 4.

Attempts were made to use a Chromebook and UDP forwarding apps for Chrome or the linux container but were not successful.

[I originally posted this procedure to the MCPE subreddit about a year ago](https://www.reddit.com/r/MCPE/comments/lepeu4/connect_xbox_minecraft_to_a_bedrock_server_over/).