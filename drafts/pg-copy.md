Copy production data to a review app to test your database migrations.

---

```sh
> heroku addons:create heroku-postgresql:hobby-basic -a hottips-pr-100

# Create a fresh new database in your review app (or staging... wherever).
# Replace "hottips-pr-100" with the name of your target app.
```

---

```sh
> heroku pg:copy hottips-prod::DATABASE_URL HEROKU_POSTGRESQL_GREEN_URL \
>   -a hottips-pr-100


# This copies your production database into the database you just created.
# Replace GREEN with the new database color (see output from addons:create).
# Replace "hottips-prod" with your production app name.
```

---

```sh
> heroku pg:promote HEROKU_POSTGRESQL_GREEN_URL -a ike-production-pr-826

# This makes your new database the primary database for your app.
# It runs your release phase command if you have one (which runs migrations
# in my case). If you don't, you can `heroku run bash` and kick of your
# migrations manually.
```

---

https://devcenter.heroku.com/articles/upgrading-heroku-postgres-databases#upgrading-with-pg-copy
