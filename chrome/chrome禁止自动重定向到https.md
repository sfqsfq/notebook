[chrome禁止自动重定向到https](https://support.google.com/chrome/thread/162605516/accessing-http-localhost-with-chrome-100?hl=en)





As of 2020, Chrome automatically redirects all HTTP URLs to HTTPS. You can stop Chrome from automatically redirecting HTTP URLs to HTTPS for localhost by doing the following:

1. Go to chrome://net-internals/#hsts
2. Enter "localhost" (without quotes) in the box underneath "delete domain security policies"
3. Click Delete
4. Try accessing localhost again

This only changes the setting for the localhost domain. It does not compromise security for other domains.
