# Media Temple Amazon S3 Backup for DV (this works on other Linux servers too)


I’ve been using Media Temple for hosting and overall it has been a very pleasant experience. There are some things that are a little inconvenient, especially things like no native support for a Media Temple Amazon S3 backup. You can’t blame Media Temple for that though… it’s mainly a Plesk thing. Fortunately enough, Plesk stays enough out of your way where you can set this up on your own. We can utilize free tools from Amazon to automate backups from MediaTemple DV 4.5 servers.

```
**Disclaimer**

Media Temple recently has shifted their policies to frown upon root access. They don’t care too much about it, they just have been a little more stingy recently with giving you support on things that are not native with Plesk (understandable). Be sure that you make a backup of everything through their Snapshot tool or the native Plesk Backup Manager before moving forward.
```

