---
layout: post
title:  "Setting Up a Jekyll Blog"
date:   2018-12-02 18:15:05 -0700
categories: technical
---

I have been putting off the small chore of moving blog content from Medium.  I was curious about some of the basic, markdown based content management systems out there and decided to use Jekyll because of my familiarity with GitHub Pages.

I am a big believer in not over-engineering solutions to problems.  Before spending money on a website, check to see if a [free option through Github](https://pages.github.com/) would suffice.  

Markdown syntax is accessible to less technical users.  Everything else is just configuration.

To get started, install Ruby, Bundler, and other dependencies until your terminal doesn't scream at you when you follow [the Jekyll tutorial](https://jekyllrb.com/tutorials/home/).

![Jekyll Logo](/assets/jekyll_software_logo.png)

## Choosing a Theme

Themes aren't as easy to swap in and out as you are led to believe.  Each has different layouts they provide.  To minimize configuration, start with a theme which works for you.  Here is a [list of themes supported by GitHub pages](https://pages.github.com/themes/).

There are two factors to consider when picking a theme.  First, does the navigation suit your needs?  For a blog, you would want a landing page which showed previews of your latest blog.  For a simple page for a software download, you want the landing page to be a page.  Think about your navigation and URL structure. Second, does the theme offer all of the layouts you need?  Slate, for example, doesn't have a 'home' layout.  A site which worked on 'minima' won't necessarily work with another theme.  What matters is that a theme offers various layouts for flexibility.

When you create a page in markdown, you put metadata at the top about how you want Jekyll to handle the page.

```
---
layout: post
title:  "Setting Up a Jekyll Blog"
date:   2018-12-03 18:15:05 -0700
categories: technical
---
```

The layout referenced must exist in your `_layouts` directory.  Themes provided through gems will hide these layouts.

To overwrite them:
* create your own `_layouts` directory in the root of your project
* copy and paste the file from the source code
* make edits

## Customize your CSS

To avoid looking too basic, take a moment to customize your CSS.  Choose a different font, change a header color, anything to make your site look less than boilerplate.  Simple is good.  Too simple is lazy.  Configuring CSS is the same in concept as customizing layouts.

To configure CSS for most themes:
* create your own `assets` directory in the root of your project
* create a file `main.scss`, ideally just importing your theme
* make edits

```
---
# Only the main Sass file needs front matter (the dashes are enough)
---

@import "minima";

.site-header {
  background-color: red;
}

.site-title, .page-link {
  color: white !important;
}
```

## Adding Additional Pages

To add additional pages to your root navigation, create files in the root of your project. The `about.markdown` file in the root of most themes is an example of this working.

Jekyll is made for rendering simple pages. It is capable of much more, but spending too much time trying to make it work for your specific need is a mistake.  In my opinion, Jekyll should be used for standard site templates; blogs, personal sites, and informational pages.

## Other Notes

Starting Jekyll should be as simple as `bundle exec jekyll serve`.

Depending on your machine, you might have to configure the port in the `_config.yml` file. The default is port 4000.

`port: 3999`

Some configuration is done in `_layouts`, some configuration is done in `_includes`.  The `_includes` directory contains reusable HTML fragments which are *included* in layouts.

If you want to update your `header.html`, for example, you would:
* copy the source file from your theme into the `_includes` directory
* start editing

In summation, my experience getting started with Jekyll was a positive one.  Github has stellar documentation for pages, themes, and Jekyll.  Jekyll's documentation is slightly more cryptic but also helpful for understanding the basic concepts of the framework.
