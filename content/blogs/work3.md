+++
showonlyimage = false
draft = false
image = "img/portfolio/content3/font-awesome.jpg"
date = "2018-04-30T17:00:00+07:00"
title = "Integrate custom font svg to font-awesome"
weight = 1
+++

I have huge trouble with these stuff in my projects before.
<!--more-->

### Table of content
- [Problem](#problem)
- [Solution](#solution)

#### Problem <a name="problem"></a>

Almost my ex-projects used [Zeplin](https://zeplin.io/) to keep track designs for both QC and Developer team during project time.

The best feature of Zeplin has to be the Guideline page with all the colors, icons and fonts. So, it support developers reducing a lot of effort in implementation. The problem here is that we never have 1 front-end developer in project from start first RC to final RC right?

Maybe ex-front-end developer used svg tag to generate icon or some tools like [Glyphter](https://glyphter.com/) to create custom font package and put it into project. But, another Front-end developer will use another technique like [Ico moon](https://icomoon.io/).

As a result, time to time we will get different font sources in our poject and it is hard to maintain. Just back to my ex-project named [MBC](http://mbc.net/) which is very large social network for Arab. I created custom font from Zeplin by using Glyphter and after 4 months I moved to another project and this made font source maintainent hard for another frontend guy since Glyphter didn't have config file for old font which using to add new font to.

It motivates me a lot to find out another way to unique way we create font icon from Zeplin and that situation will never happen again in the future. One famous quote I love is

> always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live from **Martin Golding**.

as the only way to improve ourself is learn from mistake right?

Nowadays, We have numerous ways to add icons to a website, and I think the most popular way is to use [Font Awesome](https://fontawesome.com/icons?d=gallery).

I found a way to integrate font icon (SVG file) from Zeplin to Font Awesome library in pmy project is to use a tool called [Calligraphr](https://www.calligraphr.com/en/). It allows us to create some code like this

```
<i class="fa fa-lol"/>
```

and we just put all our custom font icon into a unique css file. we will never have many font source in out project during a year again.

#### Solution <a name="solution"></a>

I tried to list out most of step here.

Step 1: we login to Calligraphr site and download template for your font (I prefer img template).

![step 1][1]

Step 2: after we have template. just put image into template by photoshop or another tools. then export as img file.

![step 2][2]

Step 3: upload the image template back to Calligraphr

![step 3][3]

Step 4: at **Build font** tab, just build your font

![step 4][4]

Step 5: click to link and get file otf

![step 5][5]

Step 6: put it to your css file

```
@font-face {
    font-family: 'FontMoreAwesome';
    src: url('fonts/Font_awesome_more-Regular.otf');
    font-weight: normal;
    font-style: normal;
}

.fa-troll:before {
    font-family: FontMoreAwesome;
    content: "0";
}
```

![step 6][6]

Step 7: Enjoy your day ^^

![step 7][7]

[1]: /my-blog/img/portfolio/content3/step1.jpg
[2]: /my-blog/img/portfolio/content3/step2.jpg
[3]: /my-blog/img/portfolio/content3/step3.jpg
[4]: /my-blog/img/portfolio/content3/step4.jpg
[5]: /my-blog/img/portfolio/content3/step5.jpg
[6]: /my-blog/img/portfolio/content3/step6.jpg
[7]: /my-blog/img/portfolio/content3/step7.jpg