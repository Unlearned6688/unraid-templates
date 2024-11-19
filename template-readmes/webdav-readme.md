# WebDAV

"A simple and standalone WebDAV server." -- created by [hacdias][(https://github.com/hacdias/webdav)

## Why?

This is just a very simple, set it up in like 2 minutes WebDAV server that I found and made this (also very simple) .xml to use it in unRAID.

### Setup

**Disclaimer**: Anything beyond this .xml, such as stricter security, etc. will be found on the [creator's github](https://github.com/hacdias/webdav). 

I can't promise anything else will work. I haven't tried it. 

Also, the creator has nothing to do with my template here. If you find yourself with questions or issues, keep that in mind.

1. set the location for the config.yml file
  - defaults to the /appdata dir where docker configs are stored in unraid typically

2. add the [config.yml](https://github.com/hacdias/webdav#configuration) file to this directory.
  - note: it's a fully filled in example. your real config.yml may omit a lot of the settings!

3. set the location for your data. (defaults to a new share called "webdav."
  - I do not recommend putting this share under appdata because this is where all your data, game saves, roms, whatever you're saving, will be saved to.
  - It will take a bunch of storage space if you are actually using this webdav.
  - It doesn't have to be its own share (maybe under /data/webdav for example). It's up to you.

4. start the docker container. see if it works by logging in with a webDAV client.
