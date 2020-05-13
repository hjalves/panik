# panik
Gracefully handle application downtimes and server errors

## How it works

This project is intended to run alongside `nginx` or another reverse proxy.
Configuration example: 

```nginx
error_page 500 502 503 504 /500.html;
location /500.html {
    root /srv/http;
}

location /panik/ {
    proxy_set_header X-App-Id "XptoApp";
    proxy_pass http://unix:/run/panik.socket:/app/;
}
```

With this setup, we can run a script from `panik` app on `500.html` page, that checks
application status, read logs and refreshes the browser when it comes back online:

```html
<script src="/panik/init.js"></script>
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

- systemd (dbus) and systemd-journald integration
- (opt) docker and docker-compose integration
- websocket or wamp protocol?
- (opt) backoffice
- internal api to start / restart / shutdown applications
- check if application is healthy via http / systemd
- keep online status, last online, and other useful metrics (TBD)

## Stack (TBD)

- (op1) python 3.7+, tornado, sqlite
- (op2) python 3.7+, django + channels, sqlite
- (op3) python 3.7+, autobahn, sqlite
- (op4) go, sqlite
