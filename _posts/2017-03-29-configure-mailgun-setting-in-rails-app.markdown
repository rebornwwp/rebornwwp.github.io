---
layout: post
title:  "Configure Mailgun settings in Rails app"
date:   2017-03-29 20:15:09 +0800
comments: true
tags:
- programming
---

Recently I'm learning to create a Rails 5 api only application with an android mobile application. I want to send a activation email after user registered from the android app, and I found there is a free Mailgun add-on on Heroku. It's perfect choice to start to play with because you can send up to 100 messages/hour without being charged. The documentation is good enough but I do found several steps confusing.

Firstly, you need to create a Mailgun account and add your credit card information, otherwise you can't send to recipents other than authorized ones(which means test only). 

Then, use the test domain(XXX@XXX.mailgun.org) to ensure Mailgun not being blocked by your test mail address(In my case I use qq mail at first, it actually blocks all traffic from mailgun). After that, you need to add a custom domain. Go to your domain name provider and configure DNS settings as following the steps of Mailgun instructions. In my case I use Namecheap and it took me a while(a cup of coffee) for Mailgun to verify my domain.

After your custom domain is active, configure your development environment and make sure it works. In my case I use smtp instead of api call, the settings are for example:

```
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
	:address => "smtp.mailgun.org",
	:port => 587,
	:domain => mydomain.club',
	:user_name => "postmaster@mydomain.club",
	:password => XXXX
  }
```

Then start your rails server and see if the mail was delivered. I spent quite a few hours here because at first my emails weren't delivered. I refered to the log and showed Mailgun accepted the mail address, and one minute later a ESP throttling message showed up. The reason is Mailgun being blocked by certain ISP(like qq mail, in this case I have to use an extra mail hosting service), so I couldn't see any mail sent inbox.

If OK in development environemnt, change your production settings as well, it should work as well.

