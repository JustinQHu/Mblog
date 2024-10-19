# Mblog

## Purpose

Repo for the source files(except the static files after build) of personal blog.

1. to keep all blogs post
    static site,  all posts will be in MD file
2. to keep track of all customization to hugo and theme.
3. to enable deployment pipelineon CDN(such as Cloudflare Pages)

## Deployment

Hugo is a powerful framework for generating static sites.
Right now,  the site is built and deployed through Cloudflare Pages pipelines.

## Basic Hugo Commands

1. check and verify hugo version
   >hugo version
2. Add content
   > hugo new category/file.md
3. Start the hugo server
   1. with drafts enables
   >hugo server -D
   2. no drafts
   >hugo server
4. Build Static Pages
   > hug
