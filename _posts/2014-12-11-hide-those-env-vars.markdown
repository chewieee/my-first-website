---
layout: post
title: "Hide Those ENV VARS"
date: 2014-12-11 11:40:03 -0800
---

I was recently putting more CSS touches on my Danebook clone when I got an unexpected e-mail from Amazon. It said that my s3 account had been compromised somehow and that I racked up $2200 dollars in charges in the process!

>Dear AWS Customer,
>
>Your AWS Account is compromised! Please review the following notice and take >immediate action to secure your account.

<!-- more -->

As I scanned through the e-mail I quickly realized that it was I that had compromised my own account. When deploying to Heroku, I didn't take into account the publication of environment variables, often referred to as ENV vars. There is a handy little gem called Figaro which is extremely easy to install and use, and even ships your variables off to Heroku if you'd like!

Upon further examination I noticed that in my haste to get this clone onto the interwebs, I installed Figaro but failed to run the CLI scripts to get it set up properly. Consequently, the default application.yml was generated but the .gitignore file was not updated to ignore tracking. I loaded it up with variables and up went the application.yml, exposed to the bone.

For future reference, I'll remember that I need to do the following(per laserlemon's instructions):

Add Figaro to the Gemfile and bundle install

``` ruby
gem "figaro"
```

Install Figaro

```ruby
figaro install
```
This will create a commented config/application.yml file and add it to the .gitignore file. That's it! How did I miss this?! Now you can add your variables to your application.yml file and use them where you'd like. 

In my case, I had them called to the secrets.yml file for setting up my s3 account for image uploading.

With a deploy to Heroku, the following command prepared and shipped off my variables to the app sans exploitation:

``` ruby
figaro heroku:set -e production
```

Problem solved. Just had to remember to follow further instructions in the e-mail for rotating my access keys and deactivating the exposed one. Bada-bing, bada-boom! Now about that $2200...



