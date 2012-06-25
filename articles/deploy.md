    Title: Deploy
    Date: 26 Jun 2012
    Status: draft
    Tags: deploy, nodejs

Let's say we have remote host with some upstart-powered linux (any ubuntu since
10.04) and we want to deploy our express application. Looks like a big deal?
It's not! In most of cases it's just two commands for deploy first time:

    roco deploy:setup
    roco deploy

And exactly one command for next deploys:

    roco deploy

When something went wrong:

    roco deploy:rollback

And if you need just restart your app:

    roco deploy:start

