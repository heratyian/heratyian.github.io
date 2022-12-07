---
layout: post
title:  "How To Use Rails Credentials For User Secrets"
date:   2022-04-09 12:25:17 -0600
categories: rails credentials user secrets
---

## Why
It is considered good practice to keep sensitive information like api keys and passwords out of source control. One way to do this in rails 6 is with `rails credentials`.

## How it works
Your secrets are stored in a .yml file and encrypted in `config/credentials.yml.enc` which is tracked in source control. You call 
`EDITOR=vim rails credentials:edit` 
to use a terminal editor like vim or 
`EDITOR="code --wait" rails credentials:edit`
 to use an app like vscode. `credentials` decrypts `config/credentials.yml.enc` so it can be edited using `config/master.key`. **This file should not be tracked in source control.** 

```
development:
  aws:
    api_key: <dev-api-key-here>
production:
  aws:
    api_key: <prod-api-key-here>
```

## How to use it
You can then access your encrypted key value anywhere in your code using this syntax which handles dev, testing, and production environments.
`Rails.application.credentials[Rails.env.to_sym][:aws][:api_key]`

In order to access your credential values in production, you'll need to add config values to hosting provider. In my case, Heroku. In settings, config vars, add a new key RAILS_MASTER_KEY and set the value to the same as your master.key.
