I recently discovered lcut (part of hutils) for extracting values from Heroku router logs, or any other logs that embrace logfmt. So useful!

https://github.com/brandur/hutils

Can anyone tell me how I could include the time in the output?

---

```bash
gem install hutils

# Bummer that it's a Ruby gem, but too useful to ignore
```

---

```bash
❯ heroku logs -t -d router -r production
2018-07-07T12:39:47.878158+00:00 heroku[router]: at=info method=POST path= "/api/00000000000000/v2/reports?dyno=web.1&pid=37" host=railsautoscale.com request_id=9a259a8b-5078-40ef-a55d-df03435dacf0 fwd="54.81.61.97,173.245.52.187" dyno=web.1 connect=1ms service=8ms status=204 bytes=358 protocol=https
2018-07-07T12:39:47.960944+00:00 heroku[router]: at=info method=POST path= "/api/00000000000000/v2/reports?dyno=web.1&pid=72" host=railsautoscale.com request_id=5218705f-53b4-4e46-9bf8-2a38ee0380d9 fwd="54.211.137.200,173.245.52.149" dyno=web.1 connect=1ms service=9ms status=204 bytes=358 protocol=https
2018-07-07T12:39:48.076210+00:00 heroku[router]: at=info method=POST path= "/api/00000000000000/v2/reports?dyno=web.1&pid=11397" host=railsautoscale.com request_id=d4a095a3-f5fc-4be2-9221-cf2d9b9d7777 fwd="34.192.175.53,173.245.52.63" dyno=web.1 connect=1ms service=9ms status=204 bytes=358 protocol=https

# Typical Heroku router logs have a lot going on. Too much for me to parse at
# a glance
```

---

```bash
❯ heroku logs -t -d router | sed -l 's/path= /path=/' | lcut dyno status service code fwd method path
web.1	204	11ms		54.81.33.16,162.158.63.238	POST	/api/00000000000000/v2/reports?dyno
web.1	204	19ms		34.192.216.107,108.162.219.230	POST	/api/00000000000000/v2/reports?dyno
web.1	204	13ms		54.166.234.248,108.162.219.14	POST	/api/00000000000000/v2/reports?dyno
web.1	204	16ms		54.197.152.151,108.162.219.158	POST	/api/00000000000000/v2/reports?dyno

# Now I can tail my router logs and easily see exactly what's going on!

# I'd love to prepend the timestamp to each of these lines. If you know an easy
# way, please let me know!

# Note: Some of these logs have an unexpected space after 'path='
# (don't know why), so some cleanup with sed is necessary.
```
