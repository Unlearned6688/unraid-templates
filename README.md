## unraid-templates

## Debrid Media Bridge

##### aka

## DMB

Additional information for the DMB template
---

### !!!!ATTENTION!!!!

Riven, Zurg, and DMB (this all-in-one (AIO) project template) are ALL in alpha or beta stages currently!

This means:

> Development on any or all projects could pause, speed up, or end abruptly with no announcements from anyone at any time!

> There will absolutely be bugs and work arounds required at times

> You are effectively a tester, a guinea pig. Expect to encounter weird problems, frustrations, etc. and do not expect perfect resolutions

##### Remember, this entire project, DMB, and all the underlying projects like zurg, riven, rclone, postgresql, etc. are provided for free!!!

### !!!!/ATTENTION!!!!


## Important first time initial setup notes:
---

At the time of this writing, unraid users must do one additonal step to fix some permissions issues around docker and unraid OS

When you first run the container, after setting all the paths, variables, etc. of course, docker engine creates many of the directories using the ROOT user

This is undesirable for a multitude of reasons I won't into. Suffice to say, security is one concern, but a more pressing concern for every end user: the container won't run (properly)!

The solution is quick and painless:

stop the container. wait for it to fully stop so that anything which might be mounted is unmounted!

Solution 1

command line solution:

chown -R 99:100 /mnt/user/your/path (ex: chown -R 99:100 /mnt/user/DMB)

Solution 2

GUI solution:

1. login to your unraid root user

2. navigate to shares

3. navigate to your share with the DMB directory (ex: /mnt/user/DMB)

4. on the right side, click the + sign to get the drop down menu

5. select owner

6. set to "nobody" (unraid uses 99:100 UID:GID aka user: nobody group: users)

once you have changed the owner to "nobody", start the container once again. 

check the logs

you should see a bunch of white text scrolling by with normal start up stuff. 

eventually you'll see YELLOW and RED text popping up. don't be alarmed this is normal for a first time start

it's just letting you know that you don't have a scraper set up yet, you don't have various other things setup. this is normal!

eventually you SHOULD see "riven frontend started" along with a url like "http://0.0.0.0:3000"

go to the URL (if it's 0.0.0.0 and you're not on the unraid system, change the ip to your unraid tower's ip ex: http://192.168.1.100:3000 )

your frontend, the main interactive webpage, should be showing. congrats! wow! now just set the stuff as per normal. 

see the DMB wiki for more information or check the DMB discord or Riven github/discord for more help

(note: do not expect specialized unraid OS debugging or help. you might get help, but don't expect everyone to know the specifics with unraid OS)

(note 2: I am typically in I-am-PUID-0's discord. he assembled the DMB AIO image this template uses. if you have specific unraid-related Q's, I can (maybe) help in there (username is Justice). He is also there often. Others are as well. Stop by, chat, have fun)

#### Obtaining the Plex Token:
---

1. go to plex.tv

2. login 

3. find any AVAILABLE movie/show

4. click the three vertical dots in upper right corner of poster

5. click "view xml"

6. scroll to the end of the VERY long URL (push 'end' key for shortcut)

7. copy everything AFTER, but not including, "Plex-Token=" (it will be a long string of random characters)

8. that's your plex token. paste it.

#### Other Cool Notes to Make in Your Brain
---

If you ever get 'error 500' it probably means you either put in the wrong IP somewhere, don't have a valid Plex token, or you haven't added your DMB paths (zurg AND riven) to your Plex container!

  Map both paths to your Plex container, however only add your Riven path as a library path! If you add the Zurg rclone you are defeating the purpose of Riven to an extent

#### Notable 'bugs' at the moment!
---

(not unique to unraid) Your logs may become spammed with some sort of "websocket" error. Per the dev of Riven, this can be ignored for now! The cause is unknown, but it doesn't seem to effect performance of Riven (part of DMB, remember)

Sept 22, 2024: there is a minor bug with the frontend version GUI showing "Unknown." This is purely a visual bug and will be fixed once I get around to fiddling with the template.
