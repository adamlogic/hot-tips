Your Heroku logs are critical to troubleshooting issues and understanding the behavior of your application. Request IDs are how you correlate Heroku's router logs with your application's logs.

It's up to you to ensure your application includes the Request ID in it's log output. For newer versions of Rails, this is added by default.

---

```ruby
# config/production.rb

config.log_tags = [:request_id]

# This is set by default in Rails 5
# It works with any load balancer (or Heroku) that sets the X-Request-ID header
```

---

```shell
> heroku logs
...app[web.1]...[47ad4845-dffe-4ee2-b0d2-826ee59f6d22] Started POST...
...app[web.1]...[6f6ff5fb-9b4e-4bce-8cda-957395fee51f] Started GET...
...app[web.1]...[47ad4845-dffe-4ee2-b0d2-826ee59f6d22] Processing by...
...app[web.1]...[47ad4845-dffe-4ee2-b0d2-8d76559f6d22] Completed...
...heroku[router]...request_id=47ad4845-dffe-4ee2-b0d2-826ee59f6d22...status=200

# The request ID is how you follow a single request even though logs for
# multiple requests are interleaved.
```

---

More info:

- [Heroku docs](https://devcenter.heroku.com/articles/http-request-id)
- [Rails `log_tags` config](http://guides.rubyonrails.org/configuring.html#rails-general-configuration)
