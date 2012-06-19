    Title: My blogging engine
    Date: 21 Jun 2012
    Status: draft

This blog is my third try to write blogging engine. First two was really naive
because I tried to implement "something like..." (tumblr, blogger). It was wrong.
Keeping something not-geeky (like tumblr) in mind kills true soul of my idea.
To be honest I had no idea, but now I have.
So, what is my typical blog workflow:

1. I have an idea, I open console, I starting typing command:
2. `railway generate article Title of my idea`
3. now I have `./db/content/articles/title-of-my-idea.md` with some contents in it:

    Title: Title of my idea
    Date: 21 Jun 2012
    Status: draft

4. I write something and add this file to git: `git add db/content`
5. I run local server using `railway server` command
6. And visit http://localhost:3000/blog/title-of-my-idea
7. When I finished final tuning I push commits from `db/content` submodule
8. Post-receive hook on github triggers POST http://anatoliy.in/update
which is just `git submodule update --init` and got updated site

This flow also allows multiple authors to work on the same blog. It also easily
scales for multi-lang translations.

The structure of article is simple: beginning of file is `head` (pair of key-values
separated with colon), after blank line begins post body. First paragraph of post
body is introduction (can be printed out in index, or, maybe designed separately).
Every key listed in head accessible while rendering template.
Every site-wide variables listed in `db/content/site.yml` file and also accessible
in views and layouts.

