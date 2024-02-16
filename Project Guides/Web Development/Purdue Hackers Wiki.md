In this post, we will talk more about how we built our content mangement system 
that allows anyone to easily contribute.

Purdue Hackers Wiki is acutally using three repositories and can be found in 
this [monorepo](https://github.com/purduehackers/ph-wiki):
- [ph-wiki-site](https://github.com/purduehackers/ph-wiki-site): web project
- [ph-wiki-posts](https://github.com/purduehackers/ph-wiki-posts): repository that contains all the posts
- [ph-wiki-post-loader](https://github.com/purduehackers/ph-wiki-post-loader): node service that copy posts from ph-wiki-posts to our 
mongoDB database

## ph-wiki-site
This is simply that web application built with NextJS. Its tech stack and how to
 run it has already been discuss in readme.

## ph-wiki-posts
THis repository contains all the posts we have. Everytime someone commit or 
merge a pull request to this repository, it triggers our CI/CD pipeline and runs
 the ph-wiki-loader node service.

## ph-wiki-post-loader
In my opinion, this is the most intersting part of this project. 
ph-wiki-post-loader is a node service that utlizes Github API to fetch all the 
data from ph-wiki-posts and copy it to our MongoDB database.

Interstingly, this node service allows ph-wiki-posts to contain complicated file
 structure. For example:

[image]

This is done by recursively fetching the ph-wiki-posts repo and creating a tree 
structure in our database.

[image]

Finally let's breifly look at its tech stack:
- nodejs: JavaScript runtime environment
- express: Node.js web application framework
- octokit: help manage Github API
- github-slugger: to assign slug to each post
- mongoose: help managing our MongoDB database
- gray-matter: extract meta data from markdown files
