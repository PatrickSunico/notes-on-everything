# Heroku Deploy Preparation

#### 1. DB Preparation

When deploying to Heroku make sure to install these two gems first and specify a production group for these two gems.

```ruby
group :production do
  gem 'pg'
  gem 'rails_12factor'
end
```

Also, For best practice is to separate the development db by specifying a development group for your preferred db. Like so.

```ruby
group :development do
  gem 'mysql2', '>= 0.3.13', '< 0.5'
end
```



#### 2. The "missing `secret_token` and `secret_key_base` for 'production' environment"

Note: I'll update more tips in solving this issue but for the meantime this is a temporary fix, I wish to fix this error as cleanly and securely as possible.



I solved this problem by explicity adding the secret keybase environment variable inside of the `config/environments/production.rb`  file. With .gitignore setup to exclude the `secrets.yml`

```
config.secret_key_base = ENV["SECRET_KEY_BASE"]
```