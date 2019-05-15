---
date: "2018-04-27T15:00:00+07:00"
title: "How To Create A Blog and Deploy to Github"
tags: ["hugo", "theme", "command-line"]
---

I ve just configured out recently how to create a site and deploy it to github hosting.
<!--more-->

![My blog][1]

### Table of content

- [Introduction](#introduction)
- [Set Up Project](#setup-project)
- [Integrate Theme](#integrate-theme)
- [Deployment to Github page](#deployment)

#### Introduction <a name="introduction"></a>

Thanks to the article [Every developer should have a blog. Here’s why, and how to stick with it](https://medium.freecodecamp.org/every-developer-should-have-a-blog-heres-why-and-how-to-stick-with-it-5fd55a247fbf) that motivated me a lot to create a blog in order to share my knowledge to community.

So from that helpful topic. There are 3 things I have studied.

1. [Hugo](https://gohugo.io/)
2. [Github page](https://help.github.com/articles/what-is-github-pages/)
3. [Markdown](https://github.com/adam-p/markdown-here)

From this knowlegde I built my own site quickly and focused more on the content that I wanna share.

#### Set Up Project <a name="setup-project"></a>

My assumption is you guys already have experience with [Git](https://git-scm.com/docs/git) in command line, [Github](https://github.com/) which is a repository for your code and your labtop is **MacOS Sierra**.

We will have 3 steps to set up a project.

Step 1: Create Repo on **Github**

Repository is just a simple place to store your code. You have to create Github account first to create repository.

![Create new Repository][2]

Step 2: Install **Hugo**

```
brew install hugo
```

then cd to your Project folder and type

```
hugo new site you-site-name
cd you-site-name
git init
git remote add origin https://github.com/user-name/repo-name.git
```
Here we do 4 minor steps:

 1. Create project name you-site-name
 2. Move to your project name
 3. Initial git repo on local
 4. Point your local git to Git repo online

NOTE: a good question for newbie is where should I get this link https://github.com/user-name/repo-name.git ?

![public link to my repositoty][3]

Read this [Project Structure](https://gohugo.io/getting-started/directory-structure/) to learn the Hugo prjoject structure.

Step 3 : Push your code to **Github Repository**
```
git status
git commit -m "your-first-message"
git push origin master
```

![How your code look like on Github][4]

#### Integrate Theme <a name="integrate-theme"></a>

In this section. I gonna introduce you guys a simple way to have a perfect theme(UI - user interface) for your site.

[Hugo themes website](https://themes.gohugo.io/) a good place to start with.

we pick the theme from site above click on **Download** button to go to theme source.

![porfolio theme][5]

![porfolio theme on Github][6]

There are two ways to get the source code. But, I perfer use download as ZIP file and put it in theme folder.

![store theme to theme folder][7]

Then we must to add `publishDir = "docs"` to config.toml file since this is a way to help Github host regconize your site and public it globally.

![must do config][8]

#### Deployment to Github page <a name="deployment"></a>

This is the most imporant section that we wanna learn about. All stuff we made above is to push your site on internet.

There are so many way to push your site on internet. But, to me as a developer i perfer Github as a simple way to do. It support hosting already,

First I create a **deploy.sh** (I copied it from this [site](https://gohugo.io/hosting-and-deployment/hosting-on-github/)) file that run on terminal

```
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo -t hugo-creative-portfolio-theme-master  # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```

then I open Terminal and type `./deploy.sh` this will auto create docs folder and push your code to Github source.

![deploy command on terminal][9]

Secondly, On your Github repo. Go to **Setting** and scroll down to the bottom you will see **Github pages** section. Choose **master branch/docs folder** option.

![project setting on github][10]

DONE! click on Your site is published at [https://hoangtranson.github.io/my-blog/](Your site is published at https://hoangtranson.github.io/my-blog/) to go to your website and enjoy cup of coffee.

[1]: /my-blog/img/portfolio/content1/my-Blog.jpg
[2]: /my-blog/img/portfolio/content1/step1.jpg
[3]: /my-blog/img/portfolio/content1/step2.jpg
[4]: /my-blog/img/portfolio/content1/step3.jpg
[5]: /my-blog/img/portfolio/content1/theme_site.jpg
[6]: /my-blog/img/portfolio/content1/theme_source.jpg
[7]: /my-blog/img/portfolio/content1/place_to_store_theme.jpg
[8]: /my-blog/img/portfolio/content1/must_do_config.jpg
[9]: /my-blog/img/portfolio/content1/deploy_blog.jpg
[10]: /my-blog/img/portfolio/content1/project_setting.jpg