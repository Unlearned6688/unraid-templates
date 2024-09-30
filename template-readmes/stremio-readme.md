# Stremio-Docker for unRAID Docker Template

Adapted to unRAID OS docker from the project and settings provided by tsaridas, found [here](https://github.com/tsaridas/stremio-docker)

The purpose of this docker container is to self-host the stremio server and/or web app. It is also possible, using this container, to setup a working self-hosted stremio server *only* and then use the publicly hosted stremio [web app](https://web.stremio.com/).

## Quick and Easy Setup

The template uses the easy setup settings from tsaridas' repo. I just adapted from them the original docker-compose style to an xml designed to run as an unRAID Docker template.

You can get up and running with almost no setup.

1. Install the addon via the community addons
2. Check the network setup. You can use docker bridge like most unRAID docker setups or you can use a custom docker network.

    Two notes on this

    i. If you use the default bridge, the container will set your default streaming URL to localhost eg http://127.0.0.1:11470. The webclient doesn't like this format, though, and the fix is changing it either manually in the Stremio GUI settings to whatever your host's LAN IP is eg http://192.168.1.100 (use your own IP, obviously) OR you could add a variable to the template called ```IPADDRESS``` and give it the value of your LAN IP (not the full URL). eg 192.168.1.100. However, this tells the docker container you want to run the docker in https mode, so, I recommend either just changing the URL inside the Stremio GUI or do this next step

    ii. If you create and set a custom docker network by going into the unRAID terminal and using the command ```docker network create stremio``` (the name of the network, ```stremio```, can be whatever you want) you can then set this stremio-server container to use that network and you won't need to manually mess around inside Stremio's GUI at all. It will all "just work" upon first starting it from nothing.

3. Check the two ports required for HTTP setup. Don't change the container port, leave them as 8080 and 11470. However, you *can* change the host port numbers if you want to or need to.

4. The host path can be changed, but this is the standard /mnt/user/appdata path unRAID docker template all use. The space used by this container is *very* small and should not be an issue to leave on a smaller cache drive.

5. The variable ```NO_CORS``` should be left as ```1``` for the purposes of this easy, simple setup. If you decide change this template to use HTTPS, then this would be changed to ```0```. More information is in the original github repo on other setups beyond this simple one.

That's it. Your container should start, it should show connected to your local IP (or you can change that if you used the default docker bridge. See Step 2 above.)

## Additional Setup

**This will probably be an ever-expanding "stuff I've tried or seen" type FAQ/troubleshooting section for everything more complex than a simple ```http://ip:port``` stremio server and web app setup (The Quick and Easy Setup, above)**

Once you have a working setup that both connects in the stremio web app GUI *and* you tested that it can play videos (try a bunch just to see), then perhaps you can venture on to more "fun" things. I really must emphasize how important it is to continue forward *only* once you have verified that a simple setup works. If you try to build on a faulty foundation, then what's the point?

### Reverse Proxy

#### Intro

Maybe you're thinking "I hate using IPs and port numbers!" Or maybe people you intend to share this with hate that or you think they can't handle it, etc. Or you just like to play around with stuff.

In this section I'll go over some experiences I have personally had, tried, and used long term.

- Quick overview (with pics!) of "what works" that I have personally had up and running at some point.
- Reverse proxy of the stremio-server
- Reverse proxy of the stremio-server *and* web app
- A little bit on security, everyone's favorite topic

#### Security

Tricked you! Security goes first.

When you expose your local network applications, like stremio-server, to the internet you're making it publicly accessible. This means you have to be aware that other people, sometimes bad guys, will try to use your now-exposed network to dig their way into your local network, your server, and anything else.

A reverse proxy is a beautiful tool. Turning ugly ```http://IP:Port``` configurations into ```https://subdomain.domain.tld```? Beautiful. But, you gotta make sure only you and anyone else you allow can access the URL.

Easiest method, in my opinion as a very lazy man, is to take your fancy domain you have (if you don't then go look into that) and issue a wildcard certificate using this format ```*.local.domain.com```. An example final domain using this certificate would be ```plex.local.domain.com``` where obviously ```domain.com``` is *your* domain name and tld.

This setup is incredibly convenient for those who basically want to "lock down" all of their locally hosted web apps (of all types. plex, stremio, all of it) *but* want some of their web apps to be publicly accessible (perhaps a bitwarden container. or whatever.). So you already own a domain name, and you'd like to use that domain name again for local usage, but you don't want to deal with extra software for authentication.

Basically when you make a local domain like outlined above, you can only access the local ones when you are, well, local to the proxy host. But the proxy host, nginx proxy manager for example, still has valid https certificates. You get the nice looking URL, with only the added ```.local.``` subdomain, and if you use a VPN like a Wireguard VPN directly to your router or unRAID server (Tailscale is an implementation of Wireguard, so if you use that, just know it's the same thing).

A real life usage by me is this: I host Plex, Stremio, etc. on my unRAID server. I reverse proxy these local IPs and use my domain name with the ```.local.``` subdomain (as seen above). When I am away from home, my phone has an automation to automatically connect to my router's VPN. To my unRAID server, I am now on the local network with a local IP just like I'm at home. I can then access my Plex server, for example, exactly the way I can at home. However, if you Johnny Bad Guy tries to connect to my local-only domain, he is hit with a generic "this site doesn't exist" type error. Only someone in my local network *OR* using my VPN, which I control access to, can access and use my Plex server URL.

Got it? I won't go into more security stuff. You can look at all of the tons of stuff out there yourself. But, I'd heavily suggest considering what I discussed above. It's very slick and makes setting up new web apps nearly automatic and you don't need to reconsider security. At least not security around the newly added web app. It's still the same!

#### Reverse Proxy Setup Information

First of all, there's a bunch of ways to achieve a working URL for your stremio web app and/or server. Don't take everything here as gospel. It isn't. In fact, I suggest reading through tsardias' repo for more information. This is purely a "This worked for me. I know this works. Here, look at this thing which worked!" type of "guide." More of a display of the end product and a little bit of a "How I made it" thing. Make sense? No? Good.

Secondly, I won't be going into the official https (using port 12470) setup this docker image supports. Tsardias already covers that in their [repo](https://github.com/tsaridas/stremio-docker). Check it out if you want to know more!

My target person is basically "Guy with a domain name, guy who uses NPM (swag, traefik should also work, no reason to not work anyway), guy who has a VPN to his home network, and now just wants to self-host Stremio accessible outside his home network."

I would say your two main options, in my view, are:

1. Use the web app that is setup using this docker container and your own self-hosted stremio-server
2. Use the publicly hosted [web app](https://web.stremio.com/) on the stremio website and your own self-hosted stremio-server

##### Reverse Proxy Both the Web App and Server

Note: I've used swag, traefik, npm... all of the various reverse proxy manager docker containers. But, I currently use npm. So, that means I'm just going to speak to setting it up in npm. If you use something else, just copy over the settings. None of this is overly complicated.

1. Get the http web app/server working 100%. Test it!
2. In NPM (or whatever...) make two entries: one for the web app (I call it ```stremio.local.<DOMAIN>.<TLD>```), and one for the server (I call this ```stremio-server.local.<DOMAIN>.<TLD>```).
3. For the webapp you use whatever host port you mapped to the container port 8080, the local IP where the container is hosted, I turn on websockets (not really sure if required for this container), select your certificate, and blammo, done.
4. Do the same thing for the Server, but use port 11470 or whatever you mapped there for the host port.
![npm-stremio-server-settings](https://github.com/user-attachments/assets/f8696e25-8128-434e-967b-11652e5e0f9f)

![npm-stremio-server-settings-2](https://github.com/user-attachments/assets/2ff0533b-f85b-4067-990c-82b33940207e)

![npm-stremio-webapp-settings](https://github.com/user-attachments/assets/5c11256e-ab20-4630-bf14-5e82630baa68)

![npm-stremio-webapp-settings-2](https://github.com/user-attachments/assets/3b174708-fd8a-46ee-98c8-1bcaf0f91b30)

6. Go to ```https://stremio.local.<DOMAIN>.<TLD>``` in your favorite browser located on the same loca network as the host. It should come up and look just like it did before reverse proxying.
7. Go to the settings inside Stremio. Scroll down. Click the config wheel to change the streaming server URL to ```https://stremio-server.local.<DOMAIN>.<TLD>``` It should say "online" to indicate the connection is good.

![proxied-server-and-proxied-web](https://github.com/user-attachments/assets/3daea7cf-bcc6-4789-9f76-0f16eb638d14)

![proxied-server-and-proxied-web-2](https://github.com/user-attachments/assets/f77cc56a-b056-4c4e-81a1-e11dbecce84c)

9. Test! Watch some stuff.
10. Test your VPN! A mobile phone, using a mobile data connection -> VPN to your home network (however you accomplish that feat), is a good tester.

##### Reverse Proxy the Server and Use the Official Stremio Web App

Note: I've used swag, traefik, npm... all of the various reverse proxy manager docker containers. But, I currently use npm. So, that means I'm just going to speak to setting it up in npm. If you use something else, just copy over the settings. None of this is overly complicated.

1. Get the http web app/server working 100%. Test it!
2. In NPM (or whatever...) make one entry for the stremio-server. (I call this ```stremio-server.local.<DOMAIN>.<TLD>```).
3. You use whatever host port you mapped to the container port 11470, the local IP where the container is hosted, I turn on websockets (not really sure if required for this container), select your certificate, and blammo, done.

![npm-stremio-server-settings](https://github.com/user-attachments/assets/a87fde2a-d7b6-433f-818c-d7fbf8b72f68)

![npm-stremio-server-settings-2](https://github.com/user-attachments/assets/233be03e-79af-4144-8cb3-ed44433c8e50)

6. Go to the [web app](https://web.stremio.com/) website.
7. Go to the settings inside Stremio. Scroll down. Click the config wheel to change the streaming server URL to ```https://stremio-server.local.<DOMAIN>.<TLD>``` It should say "online" to indicate the connection is good.

![proxied-server-public-webapp](https://github.com/user-attachments/assets/3318bd80-4730-4874-bf51-26a733565bb6)

![proxied-server-and-proxied-web-2](https://github.com/user-attachments/assets/bdf33b98-6805-41ce-bbf0-0bbac503a4e5)


9. Test! Watch some stuff.
10. Test your VPN! A mobile phone, using a mobile data connection -> VPN to your home network (however you accomplish that feat), is a good tester.

#### End...for now

If you have issues, check your logs, recheck your settings (you shouldn't be changing much for the simple setup! maybe just the network mode and a port number!), read through tsardias' repo and instructions, go from there. Make sure you can get a super simple http local server running before messing with anything more complicated, in my opinion!
