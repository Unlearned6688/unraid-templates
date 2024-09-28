# Stremio-Server for unRAID Docker Template
Adapated to unRAID OS docker from the project and settings provided by tsaridas, found [here](https://github.com/tsaridas/stremio-docker)

## Quick and Easy Setup
The template uses the easy setup settings from tsaridas' repo. I just addapated from them the original docker-compose style to an xml designed to run as an unRAID Docker template.

You can get up and running with almost no setup.

1. Install the addon via the community addons
2. Check the network setup. You can use docker bridge like most unRAID docker setups or you can use a custom docker network.

    Two notes on this

    i. If you use the default bridge, the container will set your default streaming URL to localhost eg http://127.0.0.1:11470. The webclient doesn't like this format, though, and the fix is changing it either manually in the Stremio GUI settings to whatever the LAN IP is eg http://192.168.1.100 (use your own IP, obviously) OR you could add a variable to the template called ```IPADDRESS``` and give it the value of your LAN IP (not the full URL). eg 192.168.1.100. However, this tells the docker container you want to run the docker in https mode, so, I recommend either just changing the URL inside the Stremio GUI or do this next step

    ii. !!!RECOMMENDED METHOD!!! If you create and set a custom docker network by going into the unRAID terminal and using the command ```docker network create stremio``` (the name of the network, ```stremio```, can be whatever you want) you can then set this stremio-server container to use that network and you won't need to manually mess around inside Stremio's GUI at all. It will all "just work" upon first starting it from nothing.

3. Check the two ports required for HTTP setup. Don't change the container port, leave them as 8080 and 11470. However, you *can* change the host port numbers if you want to or need to.

4. The host path can be changed, but this is the standard /mnt/user/appdata path unRAID docker template all use. The space used by this container is *very* small and should not be an issue to leave on a smaller cache drive.

5. The variable ```NO_CORS``` should be left as ```1``` for the purposes of this easy, simple setup. If you decide change this template to use HTTPS, then this would be changed to ```0```. More information is in the original github repo on other setups beyond this simple one.

That's it. Your container should start, it should show connected to your local IP (or you can change that if you used the default docker bridge. See Step 2 above.)

If you have issues, check your logs, recheck your settings (you shouldn't be changing much for the simple setup! maybe just the network mode and a port number!), read through tsardias' repo and instructions, go from there. Make sure you can get a super simple http local server running before messing with anything more complicated, in my opinion!
