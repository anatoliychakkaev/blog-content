
    Title: Things Done

# RailwayJS <br/><small>MVC framework</small>

Started at Jan 2011 as holiday project. Now ready to stable release 1.0. This
framework is based on [Express](http://expressjs.org), and basically railwayjs
app is just express app with standard structure of directories, and additional
tools for debugging, profiling and generating code.

RailwayJS ideology:

- no magic, only best practices
- convention over configuration
- standards over conventions
- thin controllers

Features:

- simple routing with printing routes map
- hot code loading
- code generators
- API for extensions and generators
- debug console
- verbose logging
- number of extensions

# JugglingDB <br/><small>multidatabase ORM</small>

Started at the middle of 2011. The goal of project: provide common interface for
the most popular databases, both sql and nosql. On top of different db adapters
jugglingdb adds validations and hooks.

Now jugglingdb supports redis, mongodb, neo4j, mysql, postgres. More adapters
coming soon.

# Roco <br/><small>deployment tool</small>

When I was a ruby programmer capistrano was the best deployment solution. After
moving to nodejs I continued to use capistrano for some time, but very soon I
got tired of repeatable actions that can be automated. First thought was: I can
generate Capfile, because I have all required information in package.json. But
this solution still requires a ruby as dependency which is looks like a fifth
wheel. And I've decided to implement something similar, but more unobtrusive.

Finally I got this tiny replacement for capistrano for nodejs projects.

Deploy using roco doesn't requre any additional configuration files at all.
Minimal configuration can be done using `package.json`. For additional stuff
roco supports `Roco.coffee` scripts. Any kind of customization can be done on
different levels: system-wide on /etc/roco.coffee, user-level on ~/.roco.coffee
and project level at ./roco.coffee or ./config/roco.coffee

Goal of this project: allow do deploy your app with minimal configuration using
simple interface:

    roco deploy:setup # for initial setup
    roco deploy       # for deploy

# Makedoc <br/><small>JSDoc documentation generator</small>

1. `npm install makedoc`
2. `makedoc -d lib -o ./docs`
3. get your jsdoc API documentation generated as html files styled with
   bootstrap

# JSDoc.info <br/><small>Online JSDoc documentation generator</small>

Idea is simple: when you have nodejs project hosted on github, for example at:
https://github.com/1602/jugglingdb you can simply go to
http://jsdoc.info/1602/jugglingdb and get your API documentation immediately.

# Semicov <br/><small>test coverage report without pain</small>

