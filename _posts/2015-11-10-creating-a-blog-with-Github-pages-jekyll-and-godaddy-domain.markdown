---
layout: post
title:  "Creating a blog with GoDaddy domain, Github pages and Jekyll"
date:   2015-11-10 22:22:00
categories:
  - jekyll
  - update
  - github
---

Hi folks!

As promised in the first blog post I will write about how I put this site in place. It was fairly quick and I think it would be useful.

### 1. Github pages

Let's start with the Github pages. [The tutorial](https://pages.github.com/) is very direct and has only three steps. You just need to have an account created. After following it you should have a test website running at http://*your-user-name*.github.io.

### 2. Jekyll

At the end of the Github Pages tutorial, there's a link to [Blogging with Jekyll](https://help.github.com/articles/using-jekyll-with-pages/). I was interested in this because we need a static website to host on Github pages. [Jekyll](http://jekyllrb.com/) is a widely known static website generator and it uses some Ruby, which I feel comfortable with. You can read its tutorial then try to create a new website, tweak it and see how it looks. Once you push your changes to the repository, it's going to be built and put in place.

After creating the website, you probably want to change the theme. All the files can be referenced using relative paths, so it's easy to create HTML partials, CSS and Javascript. If you dont want to design from scratch, there are some websites with free templates. I checked [Jekyll Themes](http://jekyllthemes.org/) and [Jekyll Themes & Templates](http://jekyllthemes.io/). The later one has paid options as well.

There are three ways of working with a theme:

* Clone the repository, then change its name to *you-user-name*.github.io
* Download the theme files, copy all of them to your repository and change it
* Download the theme files, copy and paste the parts you want

>Notice that if you already have a repository with *you-user-name*.github.io, you need to delete it before renaming the cloned repository

For me it was easier to download the theme files - usually they are in a zipped package - and then copy and paste the parts I wanted to use. Most of what you need to change is in _config.yml file. Things such as title, description and author for the website are defined in this file and then used as variables on the partials.

Some themes are very complete, including the partials to enable [Google Analytics](https://analytics.google.com) and [Disqus](https://disqus.com) (for comments). I'm using both in this site. I just needed to provide the access codes and put the templates on pages.

### 3. Registering the domain

After having the Github repository set up I went to [GoDaddy](http://www.godaddy.com). The process is simple, you search for a domain you want (in my case I typed "francisrangel.com" in the box) and then they will show you if it's available and more domain options like .info, .net and others.

After selecting the domain, there are some add ons you can buy. A very interesting one is the anonymity package. What it does is to mask what the `whois` command shows. If you try the `whois` command from the command line on this site you are going to get something like:

{% highlight bash %}
Registrant Name: Registration Private
Registrant Organization: Domains By Proxy, LLC
Registrant Street: DomainsByProxy.com
Registrant Street: 14747 N Northsight Blvd Suite 111, PMB 309
Registrant City: Scottsdale
Registrant State/Province: Arizona
Registrant Postal Code: 85260
Registrant Country: United States
Registrant Phone: +1.4806242599
Registrant Phone Ext:
Registrant Fax: +1.4806242598
Registrant Fax Ext:
Registrant Email: francisrangel.com@domainsbyproxy.com
{% endhighlight %}

If you don't buy this protection or add it later, your personal details used to register the domain will be displayed. There are other options out there, but I decided to go with the one they offer when buying a domain because I didn't need to configure anything.

Next step is to make your domain to point to your Github Pages website. Following [these instructions](http://andrewsturges.com/blog/jekyll/tutorial/2014/11/06/github-and-godaddy.html) I was able to make it work. On Github Pages [tutorial page](https://pages.github.com/), there's a link at the bottom about [custom URLs](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/) where the CNAME file is explained as well.

If everything went well you should be able to see your brand new blog being opened when you hit your domain URL!

I hope that helps! Please feel free to ask question on the comments. I didn't want to enter into too much details about configurations or small steps, but I can try to answer questions in case you find some issue.

Thank you for reading this! See you soon!
