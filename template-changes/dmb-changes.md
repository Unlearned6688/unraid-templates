**Since Debrid Media Bridge and all the underlying projects are in heavy development, I'll just document my changes to the xml, when necessary, in this file for those curious. Most of the time, I'd recommmend checking the actual project (Riven or whatever) for what actually changed and why it matters or doesn't to you.**

**Mar 8, 2025**

- Made a ton of small changes so the template works with the new FastAPI merged branch in Early March, 2025

- Added Tailscale compatibility per the instructions found [here](https://docs.unraid.net/unraid-os/manual/security/tailscale/#install-tailscale-in-a-docker-container)

- I haven't tested the Tailscale implementation and thus make no promises or statements regarding its working or not. Nor do I take any responsibility for it not working. I just followed their instructions to add the ability to enable Tailscale in a Docker container.

---

**Dec 15, 2024**

- Added a more prominent "attention" banner when people download from unRAID app store.
  - I found out how to make a more "annoying" "Hey! Read this!" screen before people download for the first time. Hopefully people take heed and check some links.

- Added ```RCLONE_DIR``` optional variable to hopefully cut down on confusion regarding changing that directory.
  - As stated, this is optional. This only matters if users change the default parent directory for zurg/rclone. This tells DMB where to look. /data is the default (inside the container)

- Added ```--shm-size=128MB``` to the extra parameters in the template.
  - Users were having issues with Zilean crashing due to postgreSQL only having 64MB memory allocated by default. It requires 128MB. This fixes the issue.

---

**Dec 14, 2024**

- Added "bridge" as default network type. Again, to cut down on confusion regarding it's exclusion.
  - There was no reason for exclusion. I didn't think people would be so confused, but I was wrong, and have added it to the .xml. This was the source of a lot of issues, and I apologize for doing it. Most of all, to myself.

---

**Oct 4, 2024**

- added optional variables to enabled pgAdmin 4. added port 5050:5050 (default set to 5050 as well). this is the port pgAdmin 4 uses and has to be exposed to use it. 

- fixed some typos in the description section at the top

- added DMB as the default mount name for zurg (oops!)

---

**Oct 1, 2024**

- partially rewrote the description at the top of the template to include setup instructions so new users can just use that instead of being forced to read wikis on GitHub

---

**Sept 25, 2024, cont.**

- fixed a few syntax typos in the xml

- fixed the missing Readme portion (oops!)

- added "reasonable" default host paths for an unraid user.
  - previously these were not included because my own paths were not the typical unraid default-style ie mapping config files to appdata, etc. I changed this to bring my own personal DMB container up to date with this xml as I am satisfied with the performance of DMB with Plex and have transitioned into using as my "full time" media solution.
    
  - This makes it even easier (the goal) for newer users or those not seeking to do a lot of "setup" to jump right in using typical default pathways.
 
- "fixed" the icon...

- removed some incorrect instructions regarding plex/rclone mounts

---

**Sept 25, 2024**

- accepted some clarifications to the xml

- the bug regarding frontend version was *fixed* in one of the recent DMB image pushes. it should now work correctly.
  - (previous fix, which I was using locally but never pushed was to add a path eg ```/mnt/user/dmb/riven/frontend/version.txt``` <-> ```/frontend/riven``` . This fixed it, but was clunky, so I didn't see a need to push it to this template. This is just "for the record" for possible future reasons.

- in the spirt of our good Dr. Funke, I'm trying to "clean up" this template and repo. I'm trying to create the easiest setup for unraid users interested in riven, zurg, and this DMB AIO. I will tweak wording in the xml (the template) to better achieve that. The template was originally "just for me" and then I decided to share it for others once I saw an unfilled need amongst unraid users in the discord. Most of the tweaking is/will be related to better descriptions for paths and variabled, better explaination and linking to the correct resources to help during setup or troubleshooting, etc. along that same vein

---

**Sept 22, 2024**

- Many changes were pushed to the Riven front and backend.

- Thank you to @spants (in the discord) for pointing out some variables to add to this xml:
  - Front and backend update (true or false) and zurg update (true or false).
  - If you wanna stay on the bleeding edge of all these projects, and we all know you do!, gotta have them tasty updates right when that little container turns orange!

- Note: (small bug) Still working on figuring out how to make the Riven frontend version.txt show up correctly. Even with the file created and a path to it provided it won't update... perplexing...
The "Unknown" version is purely visual, however, so it can be ignored until that little issue is resolved in this template...
