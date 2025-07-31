1. Shutdown the existing container

2. Use either of these options if you want to backup your DUMB services (cli_debrid, riven, etc.) data BEFORE migration.

   i. In the unRAID webUI: navigate to the appdata share. Check the box next to it. Select copy. In the popup box type in something like "/mnt/user/appdata/DUMB/backup/" (without quotes). Click start. Wait. It takes a bit.

<img width="400" height="200" alt="Screenshot 2025-07-31 170715" src="https://github.com/user-attachments/assets/dc2798e3-d61b-4b59-be71-277ef4b1fa67" />

  ii. In terminal something like (make sure the directories are correct) ```mkdir /mnt/user/appdata/DUMB-backup``` then ```cp -r -P /mnt/user/appdata/DUMB/* /mnt/user/appdata/DUMB-backup/```

3. Click edit on the existing DUMB docker container. Click add a path at the bottom of the template. Add "/data" for the container path. Add "/mnt/user/appdata/DUMB/data" for the host path. Click save.

<img width="400" height="200" alt="Screenshot 2025-07-31 170754" src="https://github.com/user-attachments/assets/8e62dbaa-865b-4c9d-a885-b1661a992b4b" />

4. Start the container (this will migrate everything to /data). You can see progress in the container logs.

5. Stop the container. Click edit on the DUMB container again. Now delete all the previous paths for all the services.

<img width="600" height="400" alt="Screenshot 2025-07-31 171022" src="https://github.com/user-attachments/assets/1ced2d98-417f-4813-8f09-efa91a4d719c" />

6. Start the container - see if all is good 🤷‍♂️

7. Once you confirm everything starts and runs as it did before, you can delete the now unused directories inside /mnt/user/appdata/DUMB These are the directories that should be remaining. You can delete the other ones. Be careful not to delete one you need! (Backup can also be deleted if it exists from you making a backup before migration)

<img width="200" height="200" alt="Screenshot 2025-07-31 172838" src="https://github.com/user-attachments/assets/7512c4b6-364f-4162-a290-73429828f069" />
