Seeing H12 (request timeout) errors on Heroku? Verify your timeout settings for any library that does IO (database, disk, network).

In my experience Rack::Timeout causes more problems than it solves, but it does have excellent docs on the risks/tradeoffs: https://github.com/heroku/rack-timeout/blob/master/doc/risks.md

---

```yaml
# config/database.yml

production:
  <<: *default
  variables:
    statement_timeout: <%= ENV.fetch('DB_STATEMENT_TIMEOUT') { 3000 } %>

# This is a database-specific configuration (Postgres in this case)
```

---

```shell
# Procfile

release: DB_STATEMENT_TIMEOUT=60000 rails db:migrate
web: rails server
worker: bundle exec sidekiq

# Override your database statement timeout when running migrations or other
# long-running processes so you don't encounter unexpected timeouts.
```

---

```ruby
# some_api_wrapper.rb

def conn
  @conn ||= Faraday.new(url: BASE_URI) do |faraday|
    faraday.options[:timeout] = 3 # seconds
    # ... other faraday stuff
  end
end

# This is obviously specific to whatever HTTP library you're using.
# Hopefully you're making external calls in a background job, but either
# way, you'll want to specify a timeout.
```

More info:

- [Heroku timeouts](https://devcenter.heroku.com/articles/request-timeout)
- [Rack::Timeout](https://github.com/heroku/rack-timeout)
- [Why Rack::Timeout is dangerous](https://www.schneems.com/2017/02/21/the-oldest-bug-in-ruby-why-racktimeout-might-hose-your-server/)
- [Rack::Timeout's own docs on the risks](https://github.com/heroku/rack-timeout/blob/master/doc/risks.md)
- [Ultimate guide to Ruby timeouts](https://github.com/ankane/the-ultimate-guide-to-ruby-timeouts)
