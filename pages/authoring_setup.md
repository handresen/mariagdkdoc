---
title: Document authoring setup
keywords: documentation authoring setup
tags: [getting_started]
sidebar: mydoc_sidebar
permalink: authoring_setup.html
summary: In order to work on the Maria GDK documentation, it is essential to setup the production environment correctly. This involves git and jekyll setup as well as simple editor setup. Note that the documentation is configured to produce a Jekyll-site that is compatible with github pages.
---

# Initial setup

## Bash on ubuntu on windows
Windows 10 support a very lightweight and efficient system for running ubuntu under windows. This includes ability to run most linux programs natively, including gcc, ruby and most importantly for the purpose of authoring documents - Jekyll.

To install, follow instructions here [https://msdn.microsoft.com/en-us/commandline/wsl/install_guide](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).

## git
git and github are used for storing all document sources. When pushing to the github repo, the public documentation page is automatically updated using __github pages__. At the very least, basic understanding of __git__ is expected.

Git should be installed on the ubuntu for Windows environment. Open bash on ubuntu and type
```sudo apt-get install git```. Note that this may already be installed on newer releases of ubuntu for Windows.

Git can be installed in many ways. One way is to install the github software here [https://desktop.github.com](https://desktop.github.com/).

### Retrieve the documents from github
Create directory ```c:/Users/<username>/Documents/github```. Cd to this directory from the ubuntu bash: ```cd /mnt/c/Users/<username>/Documents/github```. Get the files by typing ```git clone https://github.com/handresen/mariagdkdoc```, cd to this directory.

## Ruby and Jekyll
Jekyll translates markdown documentation files (all files ending with .md) using templates and some other configuration files that are located in the ```mariagdkdoc``` folder. Jekyll runs on ruby, so both must be installed. The following is ripped and modified from [http://daverupert.com/2016/04/jekyll-on-windows-with-bash/](http://daverupert.com/2016/04/jekyll-on-windows-with-bash/)


### Install prerequisites
```
$ sudo -s
$ apt update
$ apt upgrade
$ apt install make gcc
$ apt install zlib1g-dev
$ apt install nodejs
```  
Note that nodejs is required in order to use its javascript engine.

### Install ruby  
The default ruby packages are outdated. It is necessary to set up an alternate package source in order to get a ruby that is more recent and able to run Jekyll.

```
$ apt-add-repository ppa:brightbox/ruby-ng
$ apt update
$ apt install ruby2.3 ruby2.3-dev ruby-switch
$ ruby -v
```  

The reported ruby version should be 2.3.xx. If another version is reported, jekyll may not run.

### Install Jekyll
Cd to mariagdkdoc folder retrieved from github. The *Gemfile* in this directory is required to setup Jekyll correctly.

```
$ gem install jekyll
$ apt install ruby-bundler
$ bundle install
```

Exit sudo shell using ctrl-d, cd to mariagdkdoc dir.


### Run Jekyll and view documentation
To generate and serve the documentation, run ```bundle exec jekyll serve --force_polling```. This will build the website and launch it on [http://localhost:4000](http://localhost:4000). The server can be kept running at all times, when changing the doc the webpage will be updated in a few seconds.

## Editor
Most text editors can be used, avoid notepad due to poor handling of charsets, BOM and line endings. Visual Studio code, Atom or Visual Studio all work well.

Visual studio code works well an both Linux and Windows [https://code.visualstudio.com/](https://code.visualstudio.com/) and includes markdown preview.

## Working with the documentation
Once all the above steps are completed, the user should be ready to make changes and additions to the Maria GDK documentation.

Start by opening Visual Studio code and open the documentation folder. This will display a structured overview of all the files in the left explorer pane.

{% include image.html file="vscode_edit_example.png" caption="Working with documents in VS code" %}

All documents except the top level ```index.md``` are located in the ```pages``` folder.

When making a new page, create it by right clicking the appropriate folder under pages and select __New File__. Note that all pages must contain a section at the top called "front matter" which is formatted in YAML. Copy a front matter section from an existing md-page and modify the contents to match the new file.

The sidebars are configured by editing the YAML-files in ```_data/sidebars```.

{% include links.html %}