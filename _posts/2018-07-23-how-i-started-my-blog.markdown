---
layout: post
title:  "How To Start a Jekyll Themed Blog Using GitHub Pages"
date:   2018-07-23 7:00:00 -0600
categories:
  - ruby
  - jekyll
  - github pages
---

Now that I've covered all the introductory content, I'd like to jump into my first technically related post. This entry is going to cover the steps taken to get this blog up and running. This requires covering several topics including:

 * [GitHub Pages - Background](#github-pages---background)
 * [Step 1: Create a User Page](#step-1-create-a-user-page)
 * [Step 2: Install Ruby](#step-2-install-ruby)
 * [Step 3: Getting Started with Jekyll](#step-3-getting-started-with-jekyll)
 * [Step 4: Customize Your Site](#step-4-customize-your-site)
 * [Step 5: Deploy Site to GitHub](#step-5-deploy-site-to-github)
 * [Step 6 (Optional): Custom Domain using Hover](#step-6-optional-custom-domain-using-hover)
 * [Wrapping Up](#wrapping-up)
 * [Sources](#sources)

# Github Pages - Background #
---

GitHub pages is an excellent platform for hosting static websites and comes free of charge as a service provided through GitHub. I used GitHub user pages to host and deploy my personal Jekyll themed blog. GitHub pages promotes using Jekyll themes to host your static sites, but allows hosting any static site files including HTML/CSS/JavaScript.

GitHub Pages is a static site hosting service and doesn't support server-side code such as, PHP, Ruby, or Python.

### Usage Limits ##

Since GitHub pages is not designed to host large, non-static sites the following usage limits apply:

 * GitHub Pages source repositories have a recommended limit of 1GB
 * Published GitHub Pages sites may be no larger than 1GB
 * GitHub Pages sites have a *soft* bandwidth limit of 100GB/mo.
 * GitHub Pages sites have a *soft* limit of 10 builds/hr.

### User, Organization, and Project Pages ###

GitHub pages come in two flavors: project pages and user/organization pages.

"Project Pages sites are connected to a specific project, and the site files live on a branch within the project repository. User and Organization Pages sites are not tied to a specific project, and the site files live in a special repository dedicated to GitHub Pages files."

In my opinion it makes since to take the route of creating a user page if hosting a personal blog.  

*Note that each user is allowed **one** user page.*

I will be walking through the process of setting up a user page with a jekyll theme throughout this tutorial, but if you'd like to read up on GitHub pages, including Organization pages and Project Pages, please take a look at [GitHub's documentation](https://help.github.com/categories/github-pages-basics/).

# Step 1: Create a User Page #
---

The first step is to set up your own user page from GitHub pages.

This blog is set up as a user page. Files for a user page must reside in the `master` branch of the site's repository. Naming of the repository must follow the convention with the GitHub account name: `<username>.github.io`

After adding the necessary files to the repository for the static site you will be able to view your static site at the following domain: `http(s)://<username>.github.io`. We will be adding files later on using Jekyll.

*User Pages sites can be built by any user account with a verified email address. They can also use deploy keys to automate the process.*


# Step 2: Install Ruby #
---

In order to use Jekyll you will need Ruby and Ruby Gems. Below is how to install Ruby depending on your environment.

### Windows: ###
When working in a Windows environment I use a terminal emulator [cmder](http://cmder.net/) which runs [git for windows](https://git-scm.com/downloads). With these already in place all you have to do to begin using Ruby/Ruby gems is download and run the Ruby installer found [here](https://rubyinstaller.org/).

During installation you should select `Add Ruby executables to your PATH` or else you will have to manually edit `PATH`.

![image-center]({{ '/images/ruby-install.png' | absolute_url }}){: .align-center}

To edit your `PATH` in Windows 10 search "Edit the system environment variables" > Select the tab *Advanced* from the *System Properties* window > Click on *Environment Variables...* > Under *System variables* select *`PATH`* > Click *Edit* > Click *New* and add the path to where Ruby was installed ex: `C:\Ruby24-x64\bin`.

After following the above instructions you should be able to open a terminal and run ruby.  
Check by running the command `$ ruby -v` to see the version.

### Linux (Ubuntu): ###
I like to work in the Ubuntu distribution of Linux. To install Ruby in Ubuntu run the following from the command line:
```
$ sudo apt-get install ruby-full
```
For other distro installation's see [this](https://www.ruby-lang.org/en/documentation/installation/) page.

After following the above instructions you should be able to open a terminal and run ruby.  
Check by running the command `$ ruby -v` to see the version.

### Mac: ###
While I do not work with MacOS the following should be an appropriate way to install Ruby on a Mac.
First install Homebrew:
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Install Ruby:
```
$ brew install ruby
```

After following the above instructions you should be able to open a terminal and run ruby.  
Check by running the command `$ ruby -v` to see the version.

# Step 3: Getting Started with Jekyll #
---
Install [Jekyll](https://jekyllrb.com/) and [Bundler](https://bundler.io/) using Ruby Gems:
```
$ gem install jekyll bundler
```

You can check to see if Jekyll and Bundler properly installed by running
```
$ jekyll -v && bundler -v
```

Create a new directory using Jekyll/Bundler. We will call this directory `blog`.
```
$ jekyll new blog
```


`cd` into your new `blog` directory and you should see the following:
```
├── temp
|   ├── _posts/ ---------------------------------------- This folder is where your posts live.
|   |   ├── yyyy-mm-dd-welcome-to-jekyll.markdown ------ Sample post with some introductory instruction.
|   ├── .gitignore ------------------------------------- Standard git .gitignore file.
|   ├── 404.html --------------------------------------- Error page.
|   ├── _config.yml ------------------------------------ The configuration settings for the site.
|   ├── about.md --------------------------------------- About page.
|   ├── Gemfile ---------------------------------------- A list of Gem dependencies.
|   ├── Gemlock.lock ----------------------------------- A list of Gems AKA packages for the project.
|   ├── index.html ------------------------------------- Home page.
```

# Step 4: Customize Your Site #
---
From here you can customize the page however you'd like.

*I am still learning how to fully customize all the features of my own blog, but there are many resources online for full page customization, to meet your heart's desires.*

### Adding a Custom Theme ###

To keep things simple I chose to find a nice Jekyll template and go from there. You can find a list of Jekyll themes that work with GitHub pages [here](https://github.com/jekyll/jekyll/wiki/themes).

Previously GitHub only supported builds of about a dozen Jekyll themes, but starting in [November of 2017](https://blog.github.com/2017-11-29-use-any-theme-with-github-pages/) GitHub released support of "any of the hundreds of community-curated themes on GitHub.com".

I decided to go with the theme [so-simple](https://github.com/mmistakes/so-simple-theme) by [mmistakes](https://github.com/mmistakes).

To get started, in your `Gemfile` replace the line `gem "jekyll", "~> 3.8.3"` with `gem "github-pages", group: :jekyll_plugins`. *Pay attention to the colons here, this tripped me up the first time through.*

Run `bundle update` to verify that all gems are installed properly.

Next, in `_config.yml` replace `theme: jekyll` with `remote_theme: mmistakes/so-simple-theme`.

From here you can have some fun playing around with the `_config.yml` file.

### Header/Footer ###
To add custom header information such as meta tags and adding a favicon create a folder `_includes` with a file called `head-custom.html` with the header code.

I have yet to test it, but I'm sure the same applies to footer information.

### About Page ###
Editing your about page is as simple as editing the markdown in the file `about.md`.

### Posting ###
You can go to the file `yyyy-mm-dd-welcome-to-jekyll.markdown` and edit the content to create your first post. Follow this same template for creating new files under the folder `_posts`.

# Step 5: Deploy Site to GitHub #
---

If you are `cd`'d into your `blog` directory, run the following code to initialize as a GitHub repository.
```
$ git init
```
Add all the files:
```
$ git add .
```

Create a commit:
```
$ git commit -m "initial commit"
```

Setup remote repository to push to:
```
$ git remote add origin https://github.com/username/username.github.io.git
```

Push changes:
```
$ git push -u origin master
```

You can now see your blog at `http://username.github.io`

# Step 6 (Optional): Custom Domain Using Hover #
---

Head to [Hover](https://www.hover.com/) to purchase a domain name. You can use any domain service provider you'd prefer, but this tutorial will walk through setting up a custom domain using Hover.

I discovered Hover through the podcast from [CodeNewbie](https://www.codenewbie.org/). Through CodeNewbie you can get a 10% discount when using promo-code `NEWBIE`. I strongly encourage checking out their podcast, especially for junior programmers, either in school or just getting started in their career.

With the discount I payed just over $11.00 for my custom domain name for one year.

After you have purchased your domain on Hover, and verified your email, go to *Your Account* > *DNS*.

1. Delete all DNS records with a "Records" value of "A".
2. Add new records with the following values:
 * **Hostname**: @, **Record Type**: A, **Value**: 192.30.252.153
 * **Hostname**: @, **Record Type**: A, **Value**: 192.30.252.154
 * **Hostname**: www, **Record Type**: CNAME, **Value**: http://username.github.io
 * **Hostname**: www, **Record Type**: CNAME, **Value**: http://username.github.io/

![image-center]({{ '/images/hover-domains.PNG' | absolute_url }}){: .align-center}

{:start="3"}  
3. Add a file `CNAME` in the root of your repository `username.github.io`. Only write one line to this file: `http://yournewdomain.com`.

This should now connect your custom Hover domain name to your new blog hosted on GitHub pages. Be patient, it may take up to 10 minutes to take effect.

# Wrapping Up #
---
There you have it. This is the process I took in getting my own personal blog up and running. I hope I've provided enough detail here for beginners and advanced programmers alike.

What I tend to find online is either a small piece of the puzzle or [vague (yet menacing)](http://nightvale.wikia.com/wiki/Vague,_Yet_Menacing,_Government_Agency) instructions on how to complete the task at hand. I hope that in my posts I can break it down enough that even a beginner (as I am) could understand how to get something accomplished. If you are a more advanced developer I hope you are able to pick out the pieces necessary for you.

As I've written in [About This Blog](http://jrcodeodyssey.com/administrative/2018/07/16/about-this-blog.html) I am not an expert and most of the information documented here has been compiled from several different sources, that I've found online while trying to get my blog running myself. To look into more details or to find the original content please see the sources below.

And hey, Thanks.

# Sources #
---
* [So Simple Theme](https://github.com/mmistakes/so-simple-theme) by [mmistakes](https://github.com/mmistakes).
* [Pointing Hover Domain to Github Pages](http://brendan-quinn.xyz/post/pointing-hover-domain-to-github-pages/)
* [Build a Website with Jekyll and GitHub Pages](https://x-team.com/blog/build-a-free-website-with-jekyll-and-github-pages/)
* [Jekyll](https://jekyllrb.com/)
* [CodeNewbie](https://www.codenewbie.org/)
* [Hover](https://www.hover.com/)
* [GitHub Pages](https://help.github.com/categories/github-pages-basics/)
* [Ruby](https://www.ruby-lang.org/en/downloads/)
* [Bundler](https://bundler.io/)

### Something Fun to Check Out ###
* [Welcome to Night Vale Podcast](http://www.welcometonightvale.com/)
