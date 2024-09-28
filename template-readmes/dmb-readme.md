# Debrid Media Bridge unRAID Docker Template

aka

## DMB

### **ATTENTION**

Riven, Zurg, and DMB (this all-in-one (AIO) project template) are ALL in alpha or beta stages currently!

This means:

- Development on any or all projects could pause, speed up, or end abruptly with no announcements from anyone at any time!

- There will absolutely be bugs and work arounds required at times

- You are effectively a tester, a guinea pig. Expect to encounter weird problems, frustrations, etc. and do not expect perfect resolutions

**Remember, this entire project, DMB, and all the underlying projects like zurg, riven, rclone, postgresql, etc. are provided for free!!!**

## Additional information for the DMB template

### Important first time initial setup notes

If you're new to using rclone, zurg, riven, DMB, or all of it, I'd suggest this basic, loose, order to set things up quickly/painlessly.

#### "Quick start" steps

1) Read through this [wiki](https://github.com/I-am-PUID-0/DMB/wiki) from I-am-PUID-0 on how to setup and an explanation of settings in DMB.

2) Read what I wrote below regarding the owner/group permissions using unRAID as the host (which you are doing if you are here). This part is important or the container won't run!

**When you have completed steps 1 and 2 your DMB container *should* be working! If not, please recheck your paths, recheck your permissions, owner, group, and if you're still having issues you can ask in the [discord](https://discord.gg/8dqKUBtbp5). If you ask a question regarding troubleshooting ALWAYS INCLUDE THE DMB LOG! Also, it's incredibly helpful to include your filled-in (redact your API keys!) docker run/template so we can see *exactly* where there might be pathing issues.**

#### "Advanced" setup (have a fully operational container before delving deeper!)

3) This is a [guide](https://rivenmedia.github.io/wiki/) from the Riven developer. I *highly* suggest giving it at least a short read through. The info is good for beginners or not-beginners alike   
   
4) Along with the above guide, I would also suggest reading [this](https://dreulavelle.github.io/rank-torrent-name/users/faq/) on how to handle ranking torrents

### Specific unRAID First Time Configuration (Step 2 in the above "Quick Start")

At the time of this writing, unraid users must do one additonal step to fix some permissions issues around docker and unraid OS

When you first run the container, after setting all the paths, variables, etc. of course, docker engine creates many of the directories using the ROOT user

This is undesirable for a multitude of reasons I won't into. Suffice to say, security is one concern, but a more pressing concern for every end user: the container won't run (properly)!

The solution is quick and painless:

stop the container. wait for it to fully stop so that anything which might be mounted is unmounted!

Command line solution:

```chown -R 99:100 /mnt/user/your/path``` (eg: ```chown -R 99:100 /mnt/user/DMB```)

GUI solution:

1. login to your unraid root user

2. navigate to shares

3. navigate to your share with the DMB directory (ex: ```/mnt/user/DMB```)

4. on the right side, click the + sign to get the drop down menu

5. select owner

6. set to "nobody" (unraid uses 99:100 UID:GID aka user: nobody group: users)

Now restart the container and check the logs for scrolling white text, then some YELLOW and RED text (normal errors on first startup).

Look for the message confirming riven frontend has started. Plex needs to start or restart when this message shows up (if you're doing that manually).

Having done the above, your frontend, the main interactive webpage, should be showing.
**Congrats! Wow!**
Now just set the riven settings as per normal. 

See the DMB wiki (above) for more information or check the DMB discord or Riven github/discord for more help

(note: do not expect specialized unraid OS debugging or help. you might get help, but don't expect everyone to know the specifics with unraid OS)

(note 2: I am typically in I-am-PUID-0's discord. he assembled the DMB AIO image this template uses. if you have specific unraid-related Q's, I can (maybe) help in there (username is Justice). He is also there often. Others are as well. Stop by, chat, have fun)

### Obtaining the Plex Token

1. go to plex.tv

2. login

3. find any AVAILABLE movie/show

4. click the three vertical dots in upper right corner of poster

5. click "view xml"

6. scroll to the end of the VERY long URL (push 'end' key for shortcut)

7. copy everything *AFTER*, but not including, "Plex-Token=" (it will be a long string of random characters)

8. that's your plex token. paste it.

### Other Cool Notes to Make in Your Brain

If you ever get 'error 500' it probably means you either put in the wrong IP somewhere, don't have a valid Plex token, or you haven't added your DMB paths (zurg AND riven) to your Plex container!

Map both paths to your Plex container, however only add your Riven path as a library path! If you add the Zurg rclone you are defeating the purpose of Riven to an extent

### Notable 'bugs' at the moment

(not unique to unraid) Your logs may become spammed with some sort of "websocket" error. Per the dev of Riven, this can be ignored for now! The cause is unknown, but it doesn't seem to effect performance of Riven (part of DMB, remember)

Sept 22, 2024: there is a minor bug with the frontend version GUI showing "Unknown." This is purely a visual bug and will be fixed once I get around to fiddling with the template.
-- **Update Sept 25, 2024 - The frontend text issue was fixed by a DMB update**
