pgweb is my favorite GUI for Postgres. It's open source, installed via Homebrew, launched via command line, and has a simple, effective UI.

http://sosedoff.github.io/pgweb/

Here are some alias-worthy one-liners I use to launch it.


```bash
brew install pgweb

# Obviously a mac-specific command, but check the GitHub page for other
# installation options.
```

---

```bash
pgweb --host localhost

# Launch pgweb connected to your local PG. You can also specify a DB via
# command line, but I just select it in the GUI.
```

---

```bash
heroku config:get DATABASE_URL | xargs pgweb --url

# Launch pgweb connected to a Heroku Postgres DB
```
