Heroku will include memory/CPU metrics in log output if you enable it. You can parse this info out of your logs using lcut, or use a tool like @librato to visualize it.

Great for troubleshooting an action that's spiking memory.

---

```bash
❯ heroku labs:enable log-runtime-metrics

# After your next deploy or restart, you'll see these metrics in all of your
# dyno logs.
```

---

```bash
❯ heroku logs -t -d web | grep sample                                
2018-07-07T13:13:07.616902+00:00 heroku[web.1]: source=web.1 dyno=heroku.62565967.7e2ee10e-fe0c-4494-8bde-38b3c461511d sample#load_avg_1m=0.02 sample#load_avg_5m=0.06 sample#load_avg_15m=0.05
2018-07-07T13:13:07.617017+00:00 heroku[web.1]: source=web.1 dyno=heroku.62565967.7e2ee10e-fe0c-4494-8bde-38b3c461511d sample#memory_total=308.66MB sample#memory_rss=272.77MB sample#memory_cache=35.89MB sample#memory_swap=0.00MB sample#memory_pgpgin=83225pages sample#memory_pgpgout=4207pages sample#memory_quota=512.00MB
```

---

```bash
❯ heroku logs -t -d web | lcut sample#memory_total sample#memory_swap
308.69MB	0.00MB
308.78MB	0.00MB
308.78MB	0.00MB

# Use lcut to hone in on the metrics you care about.
# More info: https://github.com/brandur/hutils
```

---
