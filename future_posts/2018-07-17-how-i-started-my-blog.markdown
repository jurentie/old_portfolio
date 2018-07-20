---
layout: post
title:  "How To Start a Jekyll Themed Blog Using GitHub Pages"
date:   2018-07-17 11:26:00 -0600
categories:
  - ruby
  - jekyll
  - github pages
---

Now that I've covered all the introductory content, I'd like to jump into my first technically related post. This entry is going to cover the steps taken to get this blog up and running. This requires covering several topics including:

 * [GitHub Pages - Background](#github-pages---background)
 * [Step 1: Create a User Page](#step-1-create-a-user-page)
 * [Step 2: Install Ruby](#step-2-install-ruby)
 * [Step 3: Getting Started with Jekyll]
 * [Step 4: Deploy Site to GitHub]
 * [Step 5 (Optional): Custom Domain using Hover]

# Github Pages - Background #
---

GitHub pages is an excellent platform for hosting static websites and comes free of charge as a service provided by GitHub. I used GitHub user pages to host and deploy my personal Jekyll themed blog. GitHub pages promotes using Jekyll themes to host your static sites, but allows hosting any static site files including HTML/CSS/JavaScript.

GitHub Pages is a static site hosting service and doesn't support server-side code such as, PHP, Ruby, or Python.

### Usage Limits ##

Since GitHub pages is not designed to host large, non-static sites the following usage limits apply:

 * GitHub Pages source repositories have a recommended limit of 1GB
 * Published GitHub Pages sites may be no larger than 1GB
 * GitHub Pages sites have a *soft* bandwidth limit of 100BG/mo.
 * GitHub Pages sites have a *soft* limit of 10 builds/hr.

### User, Organization, and Project Pages ###

GitHub pages come in two flavors: project pages and user/organization pages.

"Project Pages sites are connected to a specific project, and the site files live on a branch within the project repository. User and Organization Pages sites are not tied to a specific project, and the site files live in a special repository dedicated to GitHub Pages files."

In my opinion it makes since to take the route of creating a user page if hosting a personal blog.  

*Note that each user is allowed **one** user page.*

I will be walking through the process of setting up a user page with a jekyll theme throughout this tutorial, but if you'd like to read up on more about GitHub pages including Organization pages and Project Pages please take a look at [GitHub's documentation](https://help.github.com/categories/github-pages-basics/).

# Step 1: Create a User Page #
---

The first step is to set up your own user page from GitHub pages.

This blog is set up as a user page. Files for a user page must reside in the `master` branch of the site's repository. Naming of the repository must follow the convention with the GitHub account name: `<username>.github.io`

After adding the necessary files to the repository for the static site you will be able to view your static site at the following domain: `http(s):<username/orgname>.github.io`

User Pages sites can be built by any user account with a verified email address. They can also use deploy keys to automate the process.

Organization Pages sites can be built by any member with push access to the repository and a verified email address


# Step 2: Install Ruby #
---

In order to use Jekyll you will need Ruby and Ruby Gems. Below is how to install Ruby depending on your environment.

### Windows: ###
When working in a Windows environment I use a terminal emulator [cmder](http://cmder.net/) which runs [git for windows](https://git-scm.com/download/win). With these already in place all you have to do to start using Ruby/Ruby gems is to download and run the Ruby installer found [here](https://rubyinstaller.org/).

### Linux (Ubuntu): ###
When working in Linux I use the Ubuntu distribution. To install Ruby in Ubuntu run the following from the command line:
```
$ sudo apt-get install ruby-full
```
For other distro's see [this](https://www.ruby-lang.org/en/documentation/installation/) page.

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

# Step 3: Getting Started with Jekyll #
---
