+++
date = "2016-03-24T14:03:39-07:00"
draft = true
title = "Building a blog with Hugo"
author = "Aaron Kanter"
categories = ["web", "tech"]
tags = ["go", "hugo"]
description = "Thoughts and feelings about the blazing fast static site generator written in Go"
linktitle = ""
featured = ""
featuredpath = ""
featuredalt = ""
type = "post"
+++

Not to get too meta, I thought it would be appropriate to discuss my experience with [Hugo][hugo] in bringing up this blog. I generally followed the excellent [quickstart documentation](https://gohugo.io/overview/quickstart/) (which can also be conveniently hosted locally).

Overall, I like the tool but as can be expected it has some interesting features which I thought were worth pointing out. 

### Shopping Around
I did not do a tremendous amount of shopping around but I started with a few basic goals.

* [KISS](https://en.wikipedia.org/wiki/KISS_principle) - this is a blog with little user input. Additional blog posts should not require engineering work.
* Easily self-hostable
* Support comments
* Open source

This brought me to the idea of a static site -- the simplest of the simple and easily hosted anywhere (even on GitHub pages!). Sorry (not sorry) Wordpress, you're out. Comments can be outsourced to [Discourse](https://www.discourse.org), [Disqus](https://disqus.com), [Isso](https://posativ.org/isso/), or some other external service.

There seemed to be two front-runner static site generation tools: [Jekyll](https://jekyllrb.com/) and [Hugo][hugo]. Without spending too much time comparing them, I settled on Hugo because of the active community, the rich feature set (though Jekyll certainly has more), the ease of setup, and that it is written in [Go](https://www.golang.org). This last part appeals to me not only because it is [faster](https://fredrikloch.me/post/2014-08-12-Jekyll-and-its-alternatives-from-a-site-generation-point-of-view/), but also because Go is a language I've enjoyed tinkering with in the past few months.

[hugo]: https://gohugo.io/

### TOML
Configuration is used in various parts of a Hugo site from the site-wide scope all the way down to individual file headers (also called "front matter").
Hugo is the only tool I've ever used that prefers [TOML](https://github.com/toml-lang/toml) formatting (which appears to be a more fleshed-out [git-config](https://git-scm.com/docs/git-config)). Hugo does support YAML and JSON as well, but so far I've found TOML to be very clear and concise. My main concern is that it doesn't have terribly wide adoption and that the language is **still not considered stable** according to its [**author**](https://github.com/toml-lang/toml).

TOML
```
foo = "bar"
bazz = false
[container]
    nested = "inside"
    list = ["item1", "item2"]
```

YAML
```
foo: "bar"
bazz: false
container:
    nested: "inside"
    list:
        -item1
        -item2

```

#### Fun fact
Despite the clear preference for TOML throughout the Hugo [documentation](https://gohugo.io/overview/introduction/), the documentation itself only uses TOML for the site configuration and uses YAML for the front matter of each markdown file. ;)

### Markdown
Each post is generally written in Markdown, rendered by the [Blackfriday](https://github.com/russross/blackfriday) engine. You _can_ use html, rst, mmark, or asciidoc but I see no reason to use those for the average post. While this isn't particularly novel among the younger similar products, it was important enough of a feature for me to point out here.

### Themes
Hugo has a clever theme system which allows you to switch between themes of your website pretty easily. Each theme may include its own configuration, layouts, and datatypes. Generally you `git clone` the theme into the `themes` directory and specify it by name either at build time or in the configuration.

While theme aspects aren't likely to change very frequently for a blog such as this, this system makes it very easy to both share and overhaul presentation aspects without needing to change the underlying data (hopefully).

One criticism here is that version management of shared themes is rather clunky - either you remove the link to the original repository you cloned the theme from and manage it yourself (meaning you won't get any updates) or you have to deal with git submodules.

I'll also point out that I know that this capability is not unique to Hugo - a number of other blogging tools have similar theming capabilities.

### Taxonomies
One of the coolest features of Hugo are the [taxonomies](https://gohugo.io/taxonomies/overview/), which is essentially an abstraction above the usual "tags" or "categories" (those are in fact the default taxonomies) that gives you a lot more flexibility. For example, a given post might have the front matter

```toml
+++
categories = ["web", "tech"]
tags = ["hugo"]
series = ["Running a Blog"]
+++
```

Then Hugo can auto-generate pages for each taxonomy based on your provided templates. In this case, there might be a page for "Series", and each series itself could also have its own page displaying all of the posts. This makes browsing related content a _lot_ easier. Furthermore, creating and managing taxonomies is really simple. Neat!


### Integrations and Shortcodes
There are a bunch of great integrations built-in with Hugo and only require adding a line to a page or template. Two examples of this are adding Disqus commenting (`{{ template "_internal/disqus.html" . }}`) and enabling Google Analytics (`{{ template "_internal/google_analytics.html" . }}`). These are internal templates and work basically how you would expect. 

Another great integration example is embedding YouTube video (`{{</* youtube id="videocodehere" autoplay="true" */>}}`). Unlike the internal templates above, this is an example of a [shortcode](https://gohugo.io/extras/shortcodes/) (notice the angle brackets and the different keyword). One distinguishing characteristic of a shortcode is that it cannot be included in template files, but rather only on the content pages themselves. Other than that, these both serve similar purposes of extracting out more complicated features to let you focus on creating the content. Also, I probably don't need to point it out but you can create your own shortcodes and share them via themes.