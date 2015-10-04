+++
date = "2015-10-04T21:01:22+03:00"
draft = true
title = "Hosting Hugo generated site on GitHub"

+++

The reason I like Hugo so much is partly because of its great [documentation](http://gohugo.io/overview/introduction/). Yet I do believe that  [Hosting on GitHub](http://gohugo.io/tutorials/github-pages-blog/) tutorial can be improved a little bit. It contains some redundant information on how to create a blog with Hugo and concentrates on GitHub Project Pages for the most part. It does provide you with steps how to host a personal blog on GitHub User Pages, but this solution requires separate repos for source and generated content. (It took me some time to configure everything especially that I knew little about git subtrees. So I've decided to write my own article on GitHub publishing.)

## Goal
So we want to host our site on GitHub pages.. It's natural to host your web site sources in GitHub repo.
You might have played with Jekyll already. In that case you just put everything on master branch. 

The idea is to have the contents of  'public' folder on a master branch, whereas source files on some other branch. Let's call it 'source'.
Source branch is going to contain master branch inside 'public' folder. 
Using subtree. I encourage you to read more about subtrees, however I don't blame you if you don't want to (get the result quickly) here's the tutorial for you.

## Prerequisites

I assume that you have already generated a website (hugo tutorial link)

You are going to have something like this:

[hugo directory screenshot]

Public folder contains you generated website.

## Solution

I usually use SourceTree(link) for most of git related work. But unfortunately SourceTree support for subtrees is rather poor (in Windows - my primary OS). (It hangs while pushing changes to ..). Don't worry anyway. There are just a few commands you need to type in your terminal.

Commands:

Unstage everything in your brand new repository.


git rm -rf --cached .


git add README.md


git commit -m "init master branch"

git checkout --orphan source   (cannot be done in SourceTree)

rm -rf public

git add .

git commit -m "Init source branch"



git remote add origin https://github.com/tolikcode/tolikcode.github.io.git
git push origin master
git push origin source


git subtree add --prefix=public https://github.com/tolikcode/tolikcode.github.io.git master --squash


----------------------------------------------

hugo -t ThemeName

----------------------------------------------

git add -A
git commit -m "Generating site" && git push origin source
git subtree push --prefix=public https://github.com/tolikcode/tolikcode.github.io.git master



optionally you can pull 'master' branch


Now your bloggin process should be quite simple. YOu just add new blogposts, regenerate a website, 
commit this all to source branch 
and then do a 'subtree push' to master