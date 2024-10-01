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

Here are pics of my personal DMB container setup in unRAID. For the visual types.

![unraid-dmb-setup-1](https://github.com/user-attachments/assets/c11f95fa-710f-4b1d-af07-be0a81dbface)
![unraid-dmb-setup-2](https://github.com/user-attachments/assets/cb0a02b9-c986-4de8-9751-f615d48c2716)

Here is a pic of my Plex container. Note that the DMB paths are *exactly the same* in Plex and DMB (yours should be too!)

![plex-dmb-paths](https://github.com/user-attachments/assets/0f3a9286-f81d-4d24-ab9a-5cf9a5bcee25)

Here is a pic inside my Plex webUI of what exactly the path mapping looks like. Add two libraries, Movies located at /mnt/movies and Shows located at /mnt/shows. 
Do not add /data as a library!

![plex-paths1](https://github.com/user-attachments/assets/d5a450fe-33e5-465f-bce8-aca1587723d3)
![plex-paths2](https://github.com/user-attachments/assets/0af17df8-a0e0-4514-8cb2-090a1e628138)

#### "Advanced" setup (have a fully operational container before delving deeper!)

3. This is a [guide](https://rivenmedia.github.io/wiki/) from the Riven developer. I *highly* suggest giving it at least a short read through. The info is good for beginners or not-beginners alike.

4. Along with the above guide, I would also suggest reading [this](https://dreulavelle.github.io/rank-torrent-name/users/faq/) on how to handle ranking torrents.

### Specific unRAID First Time Configuration (Step 2 in the above "Quick Start")

At the time of this writing, unraid users must do one additonal step to fix some permissions issues around docker and unraid OS

When you first run the container, after setting all the paths, variables, etc. of course, docker engine creates many of the directories using the ROOT user

This is undesirable for a multitude of reasons I won't go into. Suffice to say, security is one concern, but a more pressing concern for every end user: the container won't run (properly)!

The solution is quick and painless:

FIRST stop the container. Wait for it to fully stop so that anything which might be mounted is unmounted!

Command line solution:

```chown -R 99:100 /mnt/user/your/path``` (eg: ```chown -R 99:100 /mnt/user/DMB``` or ```chown -R 99:100 /mnt/user/appdata/DMB```)

WebUI solution:

1. login to your unraid root user

2. navigate to shares

3. navigate to your share with the DMB directory (ex: ```/mnt/user/appdata/DMB```)

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

The Jellyfin and Emby integration isn't fully available yet in DMB. However, Jellyfin/Emby **will work** if you add the paths to their respective containers the same way you would for Plex. Add the Zurg ./mnt and Riven ./mnt. Add ONLY the Riven ./mnt as a library!

Pics from setups I did to test

Jellyfin

![JF-paths-in-xml](https://github.com/user-attachments/assets/d07c76d1-8ae6-40ed-9ee2-9e171cb92b0b)
![JF-paths](https://github.com/user-attachments/assets/b81f8f9b-1dc3-453c-932c-544d64595e3f)
![JF-paths2](https://github.com/user-attachments/assets/a8de3f03-f06a-4008-87bc-8786d0252ea5)

Emby

![emby-xml-settings](https://github.com/user-attachments/assets/163b358f-f7fd-4d82-8501-6fdcf0fa1993)
![emby-library-setup1](https://github.com/user-attachments/assets/b65f1f2b-8e10-4b48-88a9-cbb53b0ae1ac)

If you ever get 'error 500' it probably means you either put in the wrong IP somewhere, don't have a valid Plex token, or you haven't added your DMB paths (zurg AND riven) to your Plex container!

Map both paths to your Plex container, however only add your Riven path as a library path! If you add the Zurg rclone you are defeating the purpose of Riven to an extent

### Troubleshooting Ideas

Besides all the basic troubleshooting of rechecking paths, etc. you can also run some commands to see what is going on inside the container.
Be careful when running any commands in the terminal. You are operating as the root user. If you mistype some commands you can *BLEEP* stuff up pretty badly. Always be sure of the paths.

A useful command for checking permissions and seeing if symlinks are working correctly is
```docker exec -w /mnt/movies dmb ls -Rl```
this executes the command ls -Rl inside your doccker container named 'dmb' inside the directory called /mnt/movies aka inside the riven directory where your symlinks are located. If you renamed the container, change the dmb portion. Same if you changed the /mnt directory.
The ```ls -Rl``` command lists all the directories and files in the specified directory ```/mnt/movies``` in my case here and the -R portion specifies to do so recursively aka reports all the directories and files inside the top level directories and keeps digging down to list them all. In short, this will report every single directory and every single file in the specified directory and it will provide the owner, group, the permissions the container has for read/write. You can also figure out if symlinks are operating correctly quickly. It's just all around what does the container see right now? helpful tool.

Here's some pics from *somebody's* *cough* DMB and Plex containers of a from 1980...something called Beetle. Just Beetle apparently. *cough* (Excessive blurring to protect my Bee-Hole. Not sure what all is allowed posted here!) You can see what I mean that DMB is the user inside the container (correct). My Plex image was built using abc as the user (commonly done and also correct). Groups users (correct). It shows the exact paths I specified for the containers and the symlinks back to the zurg rclone mount. At a glance, this all tells me that everything is configured correctly, symlinks are working, and if I go into Plex and play Beetle... odd name... it will play right now.

![docker-exec-dmb](https://github.com/user-attachments/assets/2c27728d-0430-4589-9d84-97351b57204d)
![docker-exec-plex](https://github.com/user-attachments/assets/a6a71177-6f8f-4b2d-8385-777d319ec24c)

Other options for troubleshooting would be mixing up and messing around with the above command (variations of -ls you can google and find tons of documentation on that basic command)
If you feel really spicy, you can even go into an interactive shell inside the docker containers and run something like midnight commander 
```docker exec -it dmb sh``` 
then once in the shell 
```apk add mc``` 
then to run midnight commander 
```mc``` 
it can be a useful little program especially if you prefer more of a graphic interface when looking into problems.

### Notable 'bugs' at the moment

(not unique to unraid) Your logs may become spammed with some sort of "websocket" error. Per the dev of Riven, this can be ignored for now! The cause is unknown, but it doesn't seem to effect performance of Riven (part of DMB, remember)

Sept 22, 2024: there is a minor bug with the frontend version GUI showing "Unknown." This is purely a visual bug and will be fixed once I get around to fiddling with the template.
-- **Update Sept 25, 2024 - The frontend text issue was fixed by a DMB update**
