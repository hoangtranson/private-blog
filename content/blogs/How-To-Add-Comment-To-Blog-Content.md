---
date: "2018-04-29T20:00:00+07:00"
title: "How To Add Commment To Blog Content"
tags: ["hugo"]
---

After Creating my awesome website to public it and I just wonder how to add comment on that.
<!--more-->

After googling some keyword and read some API document on [GoHugo](https://gohugo.io/content-management/comments/) I have some ideas come to my mind how to make it quickly and fast.

![comment section demostration][1]

### Table of content

- [Introduction](#introduction)
- [Set Up Disqus](#setup-project)
- [Integrate to your blog](#integrate-theme)

#### Introduction <a name="introduction"></a>

In this section I will show you some my research on doing this things. I will refer you some another way to do and references.

Comment on my article is a great way to communicate with anothers and understand their ideas and Anwser their questions. So I am able to improve my knowledege and widen it.

Thanks to awesome [document](https://gohugo.io/content-management/comments/) from GoHugo that help me a lot to know how I approach and do my idea in order to have comment section on my blog.

We have so many different ways to deal with this stuff such as [Disqus](https://disqus.com/), [Static Man](https://staticman.net/), [Intense Debate](https://intensedebate.com/), [Graph comment](https://graphcomment.com/en/), [Mutt](https://muut.com/) and [Isso](https://posativ.org/isso/) for person love [Django](https://www.djangoproject.com/) like me.

Also [How to install Disqus on Hugo?](https://portfolio.peter-baumgartner.net/2017/09/10/how-to-install-disqus-on-hugo/) is a good topic to know general step I gonna do.

**Disqus discussion not generated** The error I made during do this technical task. then I found a very helpful solution on Github [Disqus discussion not generated because of wrong disqus_url](https://github.com/rstudio/blogdown/issues/52#issuecomment-288407836)

#### Set Up Disqus <a name="set-up-disqus"></a>

I registered my acc on Disqus.

![my Disqus account][2]

Then I put into config.toml file
```
disqusShortname = "https-hoangtranson-github-io-my-blog"
```
![put disqus shortname to config file][3]

#### Integrate to your blog <a name="integrate-disqus"></a>

I think this is the most difficult path for those who is not familar with code. But, you guys are still able to do it by following my guide.

First you should go to theme folder and find single.html. I usually locate at _default folder or partials folder depend on each theme.

In my case, it located at _default folder.

![place to integrate disqus discussion][4]

In the photo, we see this line of code.
```
{{ template "_internal/disqus.html" . }}
```
So I create disqus.html file in _internal folder and put this code on that file.

```
<div id="disqus_thread"></div>
<script>
    (function () {
        var d = document, s = d.createElement('script');
        s.src = 'https://https-hoangtranson-github-io-my-blog.disqus.com/embed.js';
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the
    <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a>
</noscript>
```
![create disqus html file][5]

Then we see this text on your local blog

![final result after integrating comment][6]

Deploy it to Github and see the magic.

[1]: /my-blog/img/portfolio/content2/demo.jpg
[2]: /my-blog/img/portfolio/content2/disqus_acc.jpg
[3]: /my-blog/img/portfolio/content2/config_disqus.jpg
[4]: /my-blog/img/portfolio/content2/single.html.jpg
[5]: /my-blog/img/portfolio/content2/implement_disqus.jpg
[6]: /my-blog/img/portfolio/content2/final_result.jpg