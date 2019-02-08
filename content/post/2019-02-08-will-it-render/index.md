---
title: Upgrading blogdown and Hugo Academic
author: Lucas A. Meyer
date: '2019-02-08'
slug: will-it-render
categories:
  - Blogdown
  - R
tags:
  - Blogdown
  - R
image:
  caption: ''
  focal_point: ''
---

I've just updated my site to the new blogdown (0.10) and the new Hugo Academic (3.3) versions. 

## It was not trivial

My first attempt was to simply rename the old `academic` theme in the `themes` directory and download a new one. However, there
are a lot of breaking changes between version 2.4 and version 3.3. One of the main breaking changes is that now you can use page bundles
instead of a single page per post. This allows one to keep resources (such as images) together with each post. Now each post creates a new
directory. The new `hugo-academic` theme comes with a script to convert your posts to the page bundle format, it's called `update-academic.sh`. It doesn't
work on R Markdown files, but I didn't have any. Although this is described in the Hugo documentation as a breaking change, if you **don't** convert, 
apparently nothing bad happens. Maybe the Hugo people and I have a different definition of breaking.

The most relevant breaking change is that the configurations are now in a different directory. Instead of a single giant `config.toml` in the root 
directory, we have several different files under the `config` directory: `config.toml`, `params.toml`, `menus.toml`. So I followed the instructions: opened my old 
`config.toml` file in one window, and spread its configuration out into the other files. I uploaded to Netlify and was glad to see that 
the website published. 

However, `blogdown` was not down with the new changes. When I opened my site in RStudio, blogdown required the `config.toml` file to be in the root of 
the repository. I thought it would be as easy as simply moving it, but somehow, it wasn't. `blogdown` complained that my site was broken and suggested
generating a new one. There's a  [commit](https://github.com/rstudio/blogdown/commit/de0bcf501cbdc5e81a960dd8ff3228fcb6fc807f)
addressing the issue, but apparently it's just for new sites. So I decided to create a new site, because that should be easy.

## New blogdown site

My next attempt was to create a new project, install `packrat`, install `blogdown` and then start a new site. It didn't work out. When I 
tried to create a new site, `blogdown` complained that there was already content there (the packrat directory, I assume). So I decided to create
yet another new site.

## Newer blogdown site

So I created a yet another new site, now using blogdown in RStudio, using the option to use the `gcushen/hugo-academic` team. That one worked. I then moved my 
configuration files to the appropriate places (`config.toml` to root and the other `.toml` files to `config/_default`). To make things work
I moved my old `content` files to the new site - those had already converted from page to bundle using the `update-academic.sh` script.

I then created a new script, but it defaults to the old, unbundled, format. I found out that in order to use the new page bundle functionality,
one needs to set an option `options(blogdown.new_bundle=TRUE)`. I've done that and then created the new post again and it create the page bundle as expected. 
(This is actually that post).
