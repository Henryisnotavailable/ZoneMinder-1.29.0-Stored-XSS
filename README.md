# Stored XSS in ZoneMinder v1.29.0

# To reproduce

In a ZoneMinder installation:
- Go to /index.php?view=options&tab=web
- Set WEB_TITLE_PREFIX to ZM </title><script>alert(1)</script>
- Save
- Visit any page and confirm the alert appears.
