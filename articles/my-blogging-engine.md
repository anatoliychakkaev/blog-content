    Title: My blogging engine
    Subtitle: 3<sup style="text-transform: none; text-decoration: underline">rd</sup> edition
    Date: 21 Jun 2012
    Status: public
    Tags: nodejs, programming, architecture

This blog is my third try to write blogging engine. First two was really naive
because I tried to implement "something like..." (tumblr, blogger). It was wrong
way.  Keeping something not-geeky (like tumblr) in mind kills true soul of true
geeky blogging engine.

## Tools

The mix of tools I use to build this blog:

- git - for versioning and using GutHub as persistence
- markdown
- vim - for editing articles
- railwayjs (express, ejs, bootstrap) (less then 100 lines of code for now)

## Blogging workflow

My typical blogging workflow:

1. Type in console `railway generate article Title of my idea` to get
   `./db/content/articles/title-of-my-idea.md` with some contents in it:
    <pre>
    Title: Title of my idea
    Date: 21 Jun 2012
    Status: draft
    </pre>
1. I write something and add this file to git: git add db/content
1. I run local server using `railway server` command
1. And visit `http://localhost:3000/blog/title-of-my-idea`
1. When I finished final tuning I push commits from `db/content` submodule
1. Post-receive hook on github triggers `POST http://anatoliy.in/update`
which is just `git submodule update --init` and got updated site

This flow also allows multiple authors to work on the same blog.  And we doesn't
need coding for users and permissions management because all of this kindnessly
provided by github.

## Structure of document

The structure of article is simple: beginning of file is `head` (pair of
key-values separated with colon), after blank line begins post body. First
paragraph of post body is introduction (can be printed out in index, or, maybe
designed separately).  Every key listed in head accessible while rendering
template. Every site-wide variable listed in `db/content/site.yml` file and also
accessible in views and layouts.

## Customization

Any kind of customizations available. From views/layout to site structure and
behavior. When core is 100-lines long it's pretty easy to customize.

