# Mblog

## Releases

3.0.0:

1. Moving to PaperMod theme with Hugo
2. Using Giscus for commentting
3. Reorg content,  combining 'Synopsis' and 'life' to 'eassy'
4. Upgrade Hugo to latest version (0.138.0) and reorg refactor to using yaml
5. Move them override work to site level, rathen modifying theme files directly
6. new Archives page and new search page

2.0.0:

1. Moving to Hugo with Loveit theme
2. Using Disqus for commentting 

1.0.0:

1. Personal blog built on Wordpress (lost conent)

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
   > hugo


## Great Resources for PaperMod

### SetUp
https://github.com/adityatelange/hugo-PaperMod

### Customization
https://kyxie.github.io/en/blog/tech/papermod/

### Config example and explanation
https://github.com/jesse-wei/jessewei.dev-PaperMod/blob/main/config.yml

### Giscus Comments function for Hugo, powered by GitHub Discussion
https://www.softwarecraftsperson.com/posts/2024-02-04-blog-comments-using-utterances/
https://www.lixueduan.com/posts/blog/02-add-giscus-comment/
