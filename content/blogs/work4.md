+++
showonlyimage = false
draft = false
image = ""
date = "2018-04-1T22:00:00+07:00"
title = "How to merge old commits to only one commit"
weight = 2
+++

I face this problem a lot in my projects and here is the way that I resolve it.
<!--more-->

### Table of content
- [Problem](#problem)
- [Solution](#solution)

#### Problem <a name="problem"></a>

During my projects every feature I did, I created their own branch to do that feature and **git commit** a lot during implementation.

![a lot of commit][1]

As you can see from photo above. I have there commits and two of them were dummy commit. So As a developer when I finished my ticket, I wanna merge all commits into only one.

This help a lot of in building note change in Jenkin every time I deploy new code to testing environment and also I got a meaning text in commit on Github.

#### Solution <a name="solution"></a>

So I came to quite simple solution by command line

I reset to first commit on my branch

![reset to first commit][2]

Then I re-commit it again.

![general steps][3]

Quite simple huh?

[1]: /my-blog/img/portfolio/content4/img1.jpg
[2]: /my-blog/img/portfolio/content4/img2.jpeg
[3]: /my-blog/img/portfolio/content4/img3.jpeg