browser-sync is an awesome utility for live-updating your browser as you make code changes. I love it for in-browser design (with @tailwindcss). Use it to proxy an existing web app or as a static web server.

https://browsersync.io

---

```sh
> npm install -g browser-sync

# This installs `browser-sync` as a global CLI utility.
```

---

```sh
> browser-sync start \
    --proxy railsautoscale.test \
    --files app \
    --browser "google chrome"

# Start a proxy server for http://railsautoscale.test, and open it in Chrome.
# It'll watch files in my ./app directory and refresh the browser when any
# of them are changed.
```

---

```sh
> browser-sync init
> vim bs-config.js

# Create and edit a config file. Do this so you don't have to specify options
# every time you run browser-sync.

> browser-sync start -c bs-config.js      

# Start browser-sync using your config file.
```

---

```sh
> browser-sync start --server public

# If you don't have a running app, and you just have some static HTML/CSS
# you want to see in your browser, browser-sync can also act as a static
# web server.
```

---
