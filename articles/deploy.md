    Title: Deploy
    Date: 26 Jun 2012
    Status: draft
    Tags: deploy, nodejs

Let's say we have remote host with some upstart-powered linux (any ubuntu since
10.04) and we want to deploy our express application. Looks like a big deal?
It's not! In most of cases it's just two commands for deploy first time.

    roco deploy:setup
    roco deploy

And exactly one command for next deploys:

    roco deploy

When something went wrong:

    roco deploy:rollback

And if you need just restart your app:

    roco deploy:start

## Diving deeper

Of course under the hood it all slightly more complicated than two commands.
What is deploy? Deploy is process of updating application on remote server. How
deploy can be done in most common case? Via running bunch of commands on remote
server:

1. Create directory for new release
2. Clone application sources to that directory
3. Install depndencies
4. Symlink live dir to release dir
5. Restart server
6. Check application

Each step is just one comand executed vis ssh. So, all we need - some convenient
way of writing this command sequences. This is what roco basically does.

But before running normal deploy we need to prepare our remote host, so we need
couple additional steps for initial setup:

1. Create application directories
2. Write upstart script which will control application process

## Minimal Configuration

To deploy app roco needs three variables in simpliest case: application name,
source repository, remote server name. So, minimal Roco.coffee file looks like:

    set 'application', 'appname'
    set 'repository', 'railwayjs.com:anatoliy.in.git'
    set 'hosts', 'railwayjs.com'

But most of my projects have no Roco.coffee configuration file in it. How this
can be actieved? First two variables can be read from package.json file:

    {
    "name": "appname",
    "repository": { "url": "railwayjs.com:anatoliy.in.git" }
    }

Last variable set by user-wide config, because I often deploy to my single host
for experiments. User-wide config located at `~/.roco.coffee`.

## Advanced Configuration

`Roco.coffee` files allows you to customize process using three ways:

1. set configuration variables
2. add before- and after- hooks
3. rewrite deploy steps or write whole new receipts

### Variables listing

### Hooks

### Writing receipts

