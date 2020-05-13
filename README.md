# panik
Gracefully handle application downtimes and server errors

## How it works

This project is intended to run alongside `nginx` or another reverse proxy
that supports custom 5xx pages: 

```nginx
error_page 500 502 503 504 /500.html;
location /500.html {
    root /srv/http;
}
```

With this setup, we can inject javascript from `panik` app, that checks
application status, read logs and refreshes webpage when it returns online:

```html
<script src="/_panik.js"></script>
<script>
  initPanik({app: "XptoApp"});
</script>
```

Example webpage:

```
+------------------------------------+
|           + 5xx error +            |
|                                    |
|   /  Application is currently  \   |
|   \  restarting. Please wait.  /   |
|                                    |
|       Last online: 1 min ago       |
|       Logs: ~~~~~~~~~~~~~~~~       |
|             ~~~~~~~~~~~~~~~~       |
|             ~~~~~~~~~~~~~~~~       |
|                                    |
+------------------------------------+
```

## Features / Requirements

- systemd and systemd-journald integration
- (opt) docker and docker-compose integration
- websocket or wamp protocol?
- (opt) backoffice
- internal api to start / restart / shutdown applications
- check if application is healthy via http / systemd
- keep online status, last online, and other useful TBD metrics
