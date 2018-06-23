Heroku will time out a request after 30 seconds, responding with an H12 error. Your app doesn't know this, though, and will continue processing the request unless you do something to stop it.

Rack::Timeout can raise an exception to avoid these runaway requests, but it has its own problems. Supplement with library-specific timeouts for any IO (database, disk, network) to leaving your app or data in an unexpected state.

---

```ruby
# Gemfile

gem 'rack-timeout'

# This will raise an exception after 15 seconds, well before Heroku's 30s
# timeout kicks in. See the gem README for more configuration options,
# but the defaults are a good place to start.
# https://github.com/heroku/rack-timeout
```

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
