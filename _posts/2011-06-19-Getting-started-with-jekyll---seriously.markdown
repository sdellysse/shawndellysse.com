---
layout: post
title: Getting Started with Jekyll. Seriously.
---

### Geting started with Jekyll on Heroku with a custom domain name

This blog is running on [Jekyll](https://github.com/mojombo/jekyll/). I won't
go into advertising its features, but simply put, it takes a list of postings,
some html layouts to provide the look and feel, and a small amount of
configuration, processes all of that, and spits out static html pages. The
benefit of Jekyll over Wordpress and its ilk are in its simplicity.

The issue I fought tonight was, even in its simplicity, all of the so-called
"Getting Started" or "Quickstart" guides failed me. It took me a couple hours
to collect all of the information I needed to start working with Jekyll.

###My scenario is this:

- Content should not take a backseat to the engine. That shotdown
  Wordpress.
- I wanted to be able to use my standard toolkit (Vim, git) to work with
  my blog.
- I have a GoDaddy-registered domain name.
- I want free hosting that will let me use said domain name.
- The nameserver settings on my domain have gotten mangled due to attempts
  at self-hosting. (This is a niche problem, but it may help someone else.)

### The steps needed to accomplish this:

- Install it
- Setup a git repo for the blog
- Build the layouts and entry-page.
- Connect Jekyll to Rack so that it can be served by Heroku
- Connect your static Heroku "app" to your domain.

### Install it

Make sure that Ruby and RubyGems are installed on your system. I'm not going
to go into the details of setting that up here; a google search would be able
to yield much better tutorials at setting those up than I could. Note that any
of the commands printed here have only been run and tested on ArchLinux. Aside
from whether or not you need the `sudo` should be the only thing you'll really
need to change.

    sudo gem install jekyll

We're also going to install the `heroku` gem for working with the remote servers.

    sudo gem install heroku


### Setup a minimal git repo

Now it's time to create a home for this blog. I have a directory in my `$HOME`
where I keep all of my projects, unsurprisingly called `~/projects`. Now I
will make a new dir to store this blog and stuff a git repo in it.

    mkdir ~/projects/blog
    cd ~/projects/blog
    git init

For anyone who has done anything at all with git, heroku, or rails, the above
was pretty much standard practice. The difficult part I had was figuring out
what kind of directory structure was needed for Jekyll to function. For this
bare-minimum blog, here's the directory structure we'll need, with linenotes
to explain their exact meaning:

    blog/
        | _layouts/                               [1]
                  | layout.html                   [2]
                  | post.html                     [3]
        | _posts/                                 [4]
                | 2011-06-19-Post-Title.markdown
        | _config.yml                             [5]
        | index.html                              [6]

In Jekyll, each blog post will only contain the content of that post, and not
deal with the site layout at all. To do this, you can have an arbitrary number
of layouts, and these layouts can be nested. In this case, the contents of
`blog/_layouts/layout.html` is as follows:

    <!DOCTYPE HTML>
    <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
            <meta name="author" content="Shawn Dellysse" />
            <title>{{ page.title }}</title>
        </head>
        <body>
            <header>
                <h1><a href="/">Jekyll Base</a></h1>
            </header>
            <section>
                { { content }}
            </section>
        </body>
    </html>

